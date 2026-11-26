---
tags: [projekt, mcp, csharp, cykelservice, guide]
oprettet: 2026-11-24
---

# Guide: byg MCP-serveren trin for trin

Hører til [[MCP - CykelService]]. Guiden fører dig fra tom mappe til en server, som Claude Code kan bruge, med version 1 (kun læsning).

> [!info] Koden er testet
> Kodeudsnittene i afsnit 3 til 8 er bygget og kørt den 24. nov. 2026 med .NET 10, `ModelContextProtocol` 2.4.0, Dapper 2.1.100 og Npgsql 11.0.0 mod en lokal Postgres med testdata. Det, der ikke er testet, er markeret med ⚠️.

---

## 0. Sådan spørger du om hjælp

Alle afsnit har et nummer. Når du sidder fast, så skriv til Claude med nummeret og fejlen, fx:

```
Jeg er ved 6.1 i MCP-guiden (Fremtidige projekter/MCP - CykelService).
Jeg har gjort: <hvad du har gjort>
Jeg forventede: <hvad der skulle ske>
Der skete: <hvad der skete>
Fejlbesked:
<indsæt hele fejlbeskeden>
```

Gode ting at sende med:
- Hele fejlbeskeden, ikke kun første linje
- Den fil, fejlen er i (fx `Data/RepairQueries.cs`)
- Om fejlen sker ved `dotnet build`, `dotnet run`, i MCP Inspector eller i Claude Code

Spørgsmål af typen "hvorfor" er også velkomne, fx "Hvorfor bruger vi `init`-egenskaber i 5?".

### Fremskridt

- [x] 1. Overblik læst
- [x] 2. Supabase forberedt
- [x] 3. Projekt oprettet
- [x] 4. Program.cs
- [x] 5. Modeller
- [x] 6. Dataadgang
- [x] 7. Værktøjer
- [x] 8. Testet lokalt
- [x] 9. Forbundet til Claude Code
- [x] 10. Resten af version 1
- [ ] 11. Version 2

---

## 1. Overblik

### 1.1 Hvad der sker, når du stiller et spørgsmål

```
Du: "Hvilke cykler venter?"
   │
Claude Code ──(starter serveren, taler via stdin/stdout)──► CykelService.Mcp
   │                                                            │
   │  1. tools/list  → får navne, beskrivelser og parametre     │
   │  2. tools/call "open_repairs"                              │
   │                                                            ▼
   │                                                  Dapper + Npgsql
   │                                                            │
   │                                                            ▼
   │                                              Supabase Postgres (kun læse)
   │  3. får JSON tilbage og skriver et svar til dig            │
   ◄────────────────────────────────────────────────────────────┘
```

### 1.2 Lagene

| Mappe     | Ansvar                                                            | Kender til                                |
| --------- | ----------------------------------------------------------------- | ----------------------------------------- |
| `Tools/`  | Tager imod parametre fra Claude, tjekker dem og returnerer svaret | MCP-attributter og interfaces fra `Data/` |
| `Data/`   | SQL mod databasen                                                 | Dapper og Npgsql                          |
| `Models/` | De records, der sendes tilbage som JSON                           | Ingenting                                 |

Det er samme idé som i FuelPace: værktøjerne kender kun et interface, så de kan testes uden database.

---

## 2. Forbered Supabase

Alt i dette afsnit køres i **Supabase → SQL Editor**.

### 2.1 Tjek kolonnerne

```sql
select table_name, column_name, data_type, is_nullable
from information_schema.columns
where table_schema = 'public'
  and table_name in ('customers', 'bikes', 'repairs', 'repair_items')
order by table_name, ordinal_position;
```

Sammenlign med tabellen i [[MCP - CykelService]]. Skriv afvigelser ned her:

customer_contacted er customerContacted i databasen.

### 2.2 Tjek om RLS er slået til

```sql
select c.relname, c.relrowsecurity
from pg_class c
join pg_namespace n on n.oid = c.relnamespace
where n.nspname = 'public'
  and c.relname in ('customers', 'bikes', 'repairs', 'repair_items');
```

Hvis `relrowsecurity` er `true`, skal du bruge policies i 2.3. Ellers ser den nye rolle **nul rækker**, uden at der kommer en fejl.

### 2.3 Opret en rolle, der kun kan læse

```sql
create role mcp_readonly with login password 'langTilfældigMcpKode';

grant usage on schema public to mcp_readonly;
grant select on customers, bikes, repairs, repair_items to mcp_readonly;

-- Kun hvis RLS er slået til (se 2.2):
create policy mcp_readonly_select on customers    for select to mcp_readonly using (true);
create policy mcp_readonly_select on bikes        for select to mcp_readonly using (true);
create policy mcp_readonly_select on repairs      for select to mcp_readonly using (true);
create policy mcp_readonly_select on repair_items for select to mcp_readonly using (true);
```

Lav adgangskoden med fx `openssl rand -base64 24`, og gem den i din password manager.

**Test, at rollen ikke kan skrive.** Den skal give `permission denied for table repairs`:

```sql
set role mcp_readonly;
update repairs set complete = complete where false;
reset role;
```

### 2.4 Find connection string

⚠️ Ikke testet mod Supabase. Tjek det i dashboardet.

1. Supabase → **Connect** (øverst) → vælg **Session pooler**. Den virker over IPv4, og det gør den direkte forbindelse ikke altid.
2. Brugernavnet på pooleren hedder `<rolle>.<project-ref>`, altså `mcp_readonly.abcdefgh`.
3. Skriv den om til Npgsql-format:

```
Host=aws-1-eu-north-1.pooler.supabase.com;Port=5432;Database=postgres;Username=mcp_readonly.<project-ref>;Password=<adgangskode>;SSL Mode=Require
```

Den må **aldrig** komme i git. I 9.1 lægger du den i en miljøvariabel.

---

## 3. Opret projektet

### 3.1 Kommandoer

```bash
mkdir -p ~/Code/CykelService.Mcp && cd ~/Code/CykelService.Mcp
git init
dotnet new gitignore
dotnet new console -n CykelService.Mcp -o .

dotnet add package ModelContextProtocol --version 2.4.0
dotnet add package Microsoft.Extensions.Hosting
dotnet add package Npgsql
dotnet add package Dapper

mkdir Models Data Tools
rm Program.cs   # du skriver en ny i 4
```

### 3.2 Pakkerne

| Pakke | Hvad den bruges til |
|---|---|
| `ModelContextProtocol` | MCP-serveren, attributterne til værktøjer og stdio-transporten |
| `Microsoft.Extensions.Hosting` | Den samme host og dependency injection som i ASP.NET Core |
| `Npgsql` | Forbindelsen til Postgres |
| `Dapper` | Kører SQL og fylder svaret ind i C#-objekter. Det er lettere end EF Core, når du kun læser. |

> [!question] Hvorfor Dapper og ikke EF Core?
> Du kender EF Core fra FuelPace, men her skriver du kun nogle få forespørgsler, og databasen ejes af appen. Med Dapper skriver du SQL'en selv og slipper for at holde en DbContext i takt med Supabase. Spørg, hvis du hellere vil bruge EF Core.

### 3.3 Mappestruktur, når du er færdig

```
CykelService.Mcp/
├── CykelService.Mcp.csproj
├── Program.cs
├── Models/
│   └── Models.cs
├── Data/
│   ├── RepairQueries.cs
│   └── CustomerQueries.cs
└── Tools/
    ├── RepairTools.cs
    └── CustomerTools.cs
```

---

## 4. Program.cs

```csharp
using CykelService.Mcp.Data;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using Npgsql;

// customer_name i SQL -> CustomerName i C#
Dapper.DefaultTypeMap.MatchNamesWithUnderscores = true;

var builder = Host.CreateApplicationBuilder(args);

// Al logning skal til stderr. Stdout er reserveret til MCP-protokollen.
builder.Logging.AddConsole(o => o.LogToStandardErrorThreshold = LogLevel.Trace);

var connectionString = builder.Configuration["CYKELSERVICE_DB"]
    ?? throw new InvalidOperationException("Miljøvariablen CYKELSERVICE_DB mangler.");

builder.Services.AddSingleton(NpgsqlDataSource.Create(connectionString));
builder.Services.AddSingleton<IRepairQueries, RepairQueries>();
builder.Services.AddSingleton<ICustomerQueries, CustomerQueries>();

builder.Services
    .AddMcpServer()
    .WithStdioServerTransport()
    .WithToolsFromAssembly();

await builder.Build().RunAsync();
```

Det, du skal lægge mærke til:
- **4.1 `LogToStandardErrorThreshold`:** Uden den skriver loggeren til stdout, og så kan Claude ikke læse svarene. Det er den mest almindelige fejl med MCP over stdio. Brug heller aldrig `Console.WriteLine`.
- **4.2 `builder.Configuration["CYKELSERVICE_DB"]`:** Hosten læser selv miljøvariabler, så du behøver ikke `Environment.GetEnvironmentVariable`.
- **4.3 `NpgsqlDataSource`:** Den holder en pulje af forbindelser og skal kun oprettes én gang, derfor `AddSingleton`.
- **4.4 `WithToolsFromAssembly()`:** Den finder alle klasser med `[McpServerToolType]` af sig selv.

---

## 5. Modeller

`Models/Models.cs`:

```csharp
namespace CykelService.Mcp.Models;

public record CustomerSummary(long Id, string Name, string PhoneNumber, string Email, IReadOnlyList<BikeSummary> Bikes);

public record BikeSummary(long Id, string Make, string Model, string Colour, string Type);

// init-egenskaber i stedet for positional records:
// så kan Dapper selv konvertere int -> long og numeric -> decimal.
public record OpenRepair
{
    public long Id { get; init; }
    public string CustomerName { get; init; } = "";
    public string PhoneNumber { get; init; } = "";
    public string Bike { get; init; } = "";
    public DateTime IntakeDate { get; init; }
    public decimal TotalPrice { get; init; }
    public bool MissingItems { get; init; }
    public bool CustomerContacted { get; init; }
    public string? Note { get; init; }
}

public record MonthRevenue
{
    public int Year { get; init; }
    public int Month { get; init; }
    public int Repairs { get; init; }
    public decimal Total { get; init; }
}
```

> [!warning] 5.1 Positional records og Dapper
> Hvis du skriver `record OpenRepair(long Id, ...)`, prøver Dapper at kalde constructoren. Så skal typerne passe **præcis**. Er `id` en `int4` i databasen, og du skriver `long`, får du fejlen "A parameterless default constructor or one matching signature ... is required". Med `init`-egenskaber konverterer Dapper selv. `CustomerSummary` og `BikeSummary` er positional, fordi du selv bygger dem i C# (se 6.2).

> [!tip] 5.2 Penge er `decimal`
> `total_price` er `numeric(10,2)` i databasen. Brug altid `decimal` til beløb, aldrig `double`.

---

## 6. Dataadgang

### 6.1 `Data/RepairQueries.cs`

```csharp
using CykelService.Mcp.Models;
using Dapper;
using Npgsql;

namespace CykelService.Mcp.Data;

public interface IRepairQueries
{
    Task<IReadOnlyList<OpenRepair>> GetOpenRepairsAsync(CancellationToken ct);
    Task<IReadOnlyList<MonthRevenue>> GetRevenueAsync(DateOnly from, DateOnly to, CancellationToken ct);
}

public class RepairQueries(NpgsqlDataSource db) : IRepairQueries
{
    public async Task<IReadOnlyList<OpenRepair>> GetOpenRepairsAsync(CancellationToken ct)
    {
        const string sql = """
            select r.id,
                   c.first_name || ' ' || c.last_name as customer_name,
                   c.phone_number,
                   b.make || ' ' || b.model as bike,
                   r.intake_date,
                   r.total_price,
                   r.missing_items,
                   r.customer_contacted,
                   r.note
            from repairs r
            join customers c on c.id = r.customer_id
            join bikes b on b.id = r.bike_id
            where r.complete = false
            order by r.intake_date
            """;

        await using var conn = await db.OpenConnectionAsync(ct);
        var rows = await conn.QueryAsync<OpenRepair>(new CommandDefinition(sql, cancellationToken: ct));
        return rows.ToList();
    }

    public async Task<IReadOnlyList<MonthRevenue>> GetRevenueAsync(DateOnly from, DateOnly to, CancellationToken ct)
    {
        const string sql = """
            select extract(year from r.delivery_date)::int  as year,
                   extract(month from r.delivery_date)::int as month,
                   count(*)::int                             as repairs,
                   coalesce(sum(r.total_price), 0)           as total
            from repairs r
            where r.complete = true
              and r.delivery_date >= @From
              and r.delivery_date <  @ToExclusive
            group by 1, 2
            order by 1, 2
            """;

        await using var conn = await db.OpenConnectionAsync(ct);
        var rows = await conn.QueryAsync<MonthRevenue>(new CommandDefinition(sql,
            new
            {
                From = from.ToDateTime(TimeOnly.MinValue),
                ToExclusive = to.AddDays(1).ToDateTime(TimeOnly.MinValue)
            },
            cancellationToken: ct));
        return rows.ToList();
    }
}
```

- **6.1.1 Parametre (`@From`):** Sæt aldrig værdier direkte ind i SQL-strengen. Parametre beskytter mod SQL-injection, også selvom det er Claude, der sender værdierne.
- **6.1.2 `ToExclusive`:** `delivery_date` har et klokkeslæt, så "til og med 30. nov." skrives som "før 1. dec.". Ellers kommer leveringer fra den 30. kl. 14 ikke med.
- **6.1.3 Omsætning tæller på `delivery_date`.** Det er et valg: pengene tælles den dag, cyklen er leveret. Vil du tælle på betalingsdatoen, findes den ikke i databasen endnu.

### 6.2 `Data/CustomerQueries.cs`

```csharp
using CykelService.Mcp.Models;
using Dapper;
using Npgsql;

namespace CykelService.Mcp.Data;

public interface ICustomerQueries
{
    Task<IReadOnlyList<CustomerSummary>> FindAsync(string query, CancellationToken ct);
}

public class CustomerQueries(NpgsqlDataSource db) : ICustomerQueries
{
    private record CustomerRow
    {
        public long Id { get; init; }
        public string FirstName { get; init; } = "";
        public string LastName { get; init; } = "";
        public string PhoneNumber { get; init; } = "";
        public string Email { get; init; } = "";
    }

    private record BikeRow
    {
        public long Id { get; init; }
        public long CustomerId { get; init; }
        public string Make { get; init; } = "";
        public string Model { get; init; } = "";
        public string Colour { get; init; } = "";
        public string Type { get; init; } = "";
    }

    public async Task<IReadOnlyList<CustomerSummary>> FindAsync(string query, CancellationToken ct)
    {
        const string customerSql = """
            select id, first_name, last_name, phone_number, email
            from customers
            where first_name || ' ' || last_name ilike @Pattern
               or phone_number like @Pattern
            order by last_name, first_name
            limit 10
            """;
        const string bikeSql = """
            select id, customer_id, make, model, colour, type
            from bikes
            where customer_id = any(@Ids)
            """;

        await using var conn = await db.OpenConnectionAsync(ct);
        var customers = (await conn.QueryAsync<CustomerRow>(new CommandDefinition(customerSql,
            new { Pattern = $"%{query}%" }, cancellationToken: ct))).ToList();
        if (customers.Count == 0) return [];

        var bikes = (await conn.QueryAsync<BikeRow>(new CommandDefinition(bikeSql,
            new { Ids = customers.Select(c => c.Id).ToArray() }, cancellationToken: ct))).ToList();

        return customers.Select(c => new CustomerSummary(
            c.Id, $"{c.FirstName} {c.LastName}", c.PhoneNumber, c.Email,
            bikes.Where(b => b.CustomerId == c.Id)
                 .Select(b => new BikeSummary(b.Id, b.Make, b.Model, b.Colour, b.Type))
                 .ToList())).ToList();
    }
}
```

- **6.2.1 To forespørgsler i stedet for én med `join`:** En kunde med tre cykler ville ellers give tre rækker. Det er nemmere at hente kunderne og så alle deres cykler på én gang med `any(@Ids)` end at samle rækkerne bagefter. Det kaldes at undgå N+1: du laver 2 kald i stedet for 1 + ét pr. kunde.
- **6.2.2 `ilike`:** Postgres' søgning uden forskel på store og små bogstaver.
- **6.2.3 `limit 10`:** Claude skal ikke have hele kunderegisteret, hvis søgningen er bred.

---

## 7. Værktøjerne

### 7.1 `Tools/RepairTools.cs`

```csharp
using System.ComponentModel;
using CykelService.Mcp.Data;
using CykelService.Mcp.Models;
using ModelContextProtocol;
using ModelContextProtocol.Server;

namespace CykelService.Mcp.Tools;

[McpServerToolType]
public class RepairTools(IRepairQueries repairs)
{
    [McpServerTool(Name = "open_repairs", ReadOnly = true)]
    [Description("Reparationer, der ikke er afsluttede endnu, ældste først. Viser om der mangler dele, og om kunden er kontaktet.")]
    public Task<IReadOnlyList<OpenRepair>> OpenRepairs(CancellationToken ct)
        => repairs.GetOpenRepairsAsync(ct);

    [McpServerTool(Name = "revenue", ReadOnly = true)]
    [Description("Omsætning for afsluttede reparationer i en periode, opdelt på måned. Beløb i DKK efter rabat.")]
    public Task<IReadOnlyList<MonthRevenue>> Revenue(
        [Description("Første dag i perioden, fx 2026-11-01")] DateOnly from,
        [Description("Sidste dag i perioden, fx 2026-11-30")] DateOnly to,
        CancellationToken ct)
    {
        if (from > to)
            throw new McpException("'from' skal ligge før 'to'.");

        return repairs.GetRevenueAsync(from, to, ct);
    }
}
```

### 7.2 `Tools/CustomerTools.cs`

```csharp
using System.ComponentModel;
using CykelService.Mcp.Data;
using CykelService.Mcp.Models;
using ModelContextProtocol.Server;

namespace CykelService.Mcp.Tools;

[McpServerToolType]
public class CustomerTools(ICustomerQueries customers)
{
    [McpServerTool(Name = "find_customer", ReadOnly = true)]
    [Description("Finder kunder på navn eller telefonnummer og viser deres cykler. Højst 10 resultater.")]
    public Task<IReadOnlyList<CustomerSummary>> FindCustomer(
        [Description("Hele eller dele af navnet eller telefonnummeret")] string query,
        CancellationToken ct)
        => customers.FindAsync(query, ct);
}
```

### 7.3 Hvad attributterne gør

| Kode | Hvad Claude ser |
|---|---|
| `[McpServerToolType]` | Gør klassen til en samling værktøjer, som `WithToolsFromAssembly()` finder |
| `[McpServerTool(Name = "open_repairs")]` | Værktøjets navn |
| `ReadOnly = true` | `readOnlyHint: true`: værktøjet ændrer ingenting |
| `[Description]` på metoden | Beskrivelsen, som Claude bruger til at vælge værktøjet |
| `[Description]` på en parameter | Forklaringen på parameteren |
| `DateOnly` | Bliver til `"type": "string", "format": "date"` i skemaet |
| `CancellationToken` | Bliver ikke vist til Claude, men SDK'en sender den med |
| Constructor-parametre | Kommer fra dependency injection, ligesom i en ASP.NET-controller |

Returværdien bliver lavet om til JSON af sig selv, med camelCase.

### 7.4 Skriv gode beskrivelser

Claude vælger værktøj ud fra beskrivelsen alene. Skriv:
- **hvad** den returnerer ("Reparationer, der ikke er afsluttede endnu")
- **i hvilken rækkefølge og hvor mange** ("ældste først", "højst 10")
- **enheder** ("beløb i DKK efter rabat")
- **formater** på parametre med et eksempel ("fx 2026-11-01")

### 7.5 Fejl

| Hvad der sker | Hvad Claude ser |
|---|---|
| Du kaster `McpException("...")` | `An error occurred invoking 'revenue': 'from' skal ligge før 'to'.` |
| Alle andre undtagelser, fx forkert adgangskode | `An error occurred invoking 'open_repairs'.` |

Brug `McpException` til fejl, som Claude kan rette selv, fx forkerte datoer. Andre fejl skjules for Claude, så detaljer om databasen ikke slipper ud. De kan ses i loggen på stderr (se 12).

---

## 8. Test lokalt

### 8.1 Byg

```bash
dotnet build
```

### 8.2 En testdatabase i Docker

Så kan du teste uden at røre ved dine rigtige kunder. Gem som `test/seed.sql`:

```sql
create table customers (id int generated always as identity primary key, first_name text not null, last_name text not null, street_name text not null default '', street_number text not null default '', postal_code text not null default '', city text not null default '', email text not null default '', phone_number text not null default '', club text, note text);
create table bikes (id int generated always as identity primary key, customer_id int references customers(id), make text, model text, colour text, type text);
create table repairs (id int generated always as identity primary key, bike_id int references bikes(id), customer_id int references customers(id), total_price numeric(10,2) not null default 0, discount_type text, discount_value numeric(10,2) not null default 0, note text, created_at timestamptz default now(), intake_date timestamptz not null, delivery_date timestamptz, complete bool default false, payment_recieved bool default false, customer_contacted bool default false, missing_items bool default false);

insert into customers (first_name, last_name, phone_number, email) values
  ('Jens', 'Hansen', '20112233', 'jens@example.com'),
  ('Mette', 'Larsen', '30445566', 'mette@example.com');
insert into bikes (customer_id, make, model, colour, type) values
  (1, 'Kildemoes', 'City', 'Sort', 'city'),
  (2, 'Trek', 'Marlin 5', 'Rød', 'mtb');
insert into repairs (bike_id, customer_id, total_price, intake_date, delivery_date, complete, missing_items, note) values
  (1, 1, 450,  '2026-11-02', null,         false, true,  'Venter på bremsekabel'),
  (2, 2, 1200, '2026-11-05', '2026-11-08', true,  false, null),
  (1, 1, 300,  '2026-10-10', '2026-10-12', true,  false, null);

-- Til bike_history i 10.2
create table repair_items (id int generated always as identity primary key, repair_id int references repairs(id), title text not null, price numeric(10,2) not null default 0, quantity int not null default 1, total numeric(10,2) not null default 0, discount_type text, discount_value numeric(10,2) not null default 0);

insert into repair_items (repair_id, title, price, quantity, total) values
  (1, 'Bremsekabel',  150, 1, 150),
  (1, 'Arbejdsløn',   300, 1, 300),
  (2, 'Kæde',         350, 2, 700),
  (2, 'Service',      500, 1, 500),
  (3, 'Lapning',      150, 2, 300);

create role mcp_readonly with login password 'laese-kode';
grant usage on schema public to mcp_readonly;
grant select on customers, bikes, repairs, repair_items to mcp_readonly;
```

Tabellerne er lavet til test ud fra appens kode. Ret dem til, så de passer med det, du fandt i 2.1. Linjerne i `repair_items` summerer til `total_price` på hver reparation (450, 1200 og 300), så du kan se, om `bike_history` regner rigtigt.

⚠️ `repair_items` er tilføjet bagefter og ikke kørt sammen med resten.

```bash
docker run -d --rm --name cykel-pg -e POSTGRES_PASSWORD=test -p 55432:5432 postgres:17
sleep 3
docker exec -i cykel-pg psql -U postgres < test/seed.sql

export CYKELSERVICE_DB="Host=localhost;Port=55432;Database=postgres;Username=mcp_readonly;Password=laese-kode"
```

Stop den igen med `docker stop cykel-pg`. Den bliver slettet af sig selv, så du starter forfra hver gang.

### 8.3 MCP Inspector

Inspector er en lille webside, hvor du kan se og kalde værktøjerne uden Claude:

```bash
npx @modelcontextprotocol/inspector dotnet run --project .
```

Tryk **Connect** → **Tools** → **List Tools**, og kør `open_repairs`. Med testdata skal du få Jens Hansens reparation, som venter på et bremsekabel. `revenue` for 2026-10-01 til 2026-11-30 skal give 300 kr. i oktober og 1200 kr. i november.

⚠️ Inspector er ikke testet her. Hvis miljøvariablen ikke kommer med, så sæt den under **Environment Variables** i venstre side.

### 8.4 Unit tests af værktøjerne

⚠️ Ikke kørt. Lav et testprojekt med xUnit og test værktøjsklassen med et falsk `IRepairQueries`:

```csharp
public class FakeRepairQueries : IRepairQueries
{
    public Task<IReadOnlyList<OpenRepair>> GetOpenRepairsAsync(CancellationToken ct)
        => Task.FromResult<IReadOnlyList<OpenRepair>>([]);
    public Task<IReadOnlyList<MonthRevenue>> GetRevenueAsync(DateOnly from, DateOnly to, CancellationToken ct)
        => Task.FromResult<IReadOnlyList<MonthRevenue>>([]);
}

public class RepairToolsTests
{
    [Fact]
    public void Revenue_throws_when_from_is_after_to()
    {
        var tools = new RepairTools(new FakeRepairQueries());

        Assert.Throws<McpException>(() =>
            tools.Revenue(new DateOnly(2026, 12, 1), new DateOnly(2026, 11, 1), CancellationToken.None));
    }
}
```

SQL'en tester du bedst mod testdatabasen fra 8.2. Spørg, hvis du vil sætte det op med Testcontainers.

---

## 9. Forbind til Claude Code

### 9.1 Tilføj serveren

```bash
claude mcp add cykelservice --scope user \
  -e CYKELSERVICE_DB="<connection string fra 2.4>" \
  -- dotnet run --project ~/Code/CykelService.Mcp
```

- `--scope user` gør serveren tilgængelig i alle dine projekter, og den bliver gemt i din egen config, ikke i et repo.
- `-e` lægger connection string i en miljøvariabel, så den ikke står i koden.

⚠️ Tjek flagene med `claude mcp add --help`. De kan have ændret sig.

### 9.2 Hurtigere opstart

`dotnet run` bygger hver gang, og det tager et par sekunder. Når serveren virker:

```bash
dotnet publish -c Release -o ~/.local/share/cykelservice-mcp
claude mcp remove cykelservice --scope user
claude mcp add cykelservice --scope user \
  -e CYKELSERVICE_DB="..." \
  -- ~/.local/share/cykelservice-mcp/CykelService.Mcp
```

Husk at publishe igen, når du ændrer koden.

### 9.3 Prøv det

Start Claude Code og skriv `/mcp` for at se, om `cykelservice` er forbundet. Spørg så: "Hvilke cykler venter på værkstedet?"

---

## 10. Resten af version 1: selv

Nu kender du mønstret. De to sidste værktøjer får du kun hints til, så du selv skriver koden. Spørg med afsnitsnummeret, hvis du sidder fast.

### 10.1 `unpaid_repairs`

- Samme opbygning som `open_repairs`
- `where r.complete = true and r.payment_recieved = false` (husk stavefejlen)
- Tag `delivery_date` med, så du kan se, hvor længe de har ventet

### 10.2 `bike_history`

- Parameter: `bikeId`
- Kræver `repair_items`, så husk `grant select on repair_items` i 2.3
- Hent reparationerne og så alle linjer med `repair_id = any(@Ids)`, som i 6.2
- Model: `RepairHistory` med en liste af `RepairLine` (`Title`, `Quantity`, `Price`, `Total`)
- Brug `total` og `total_price` fra databasen. Lad være med at regne priserne ud igen.
- Test med testdata fra 8.2: `bike_history` for cykel 1 (Jens' Kildemoes) skal give to reparationer, én på 450 kr. med bremsekabel og arbejdsløn, og én på 300 kr. med lapning.

### 10.3 Ekstra: `find_customer` med cykel-id'er

Når Claude har fundet en kunde, kan den bruge cykel-id'erne fra `find_customer` direkte i `bike_history`. Prøv at spørge "Hvad har jeg lavet på Jens' cykel?", og se, om Claude selv kalder begge værktøjer.

---

## 11. Version 2: skrivning

Start først, når version 1 har været i brug et stykke tid.

### 11.1 Sådan gør du

- Lav en **ny rolle** til skrivning med `update` på de få kolonner, der skal ændres, fx `grant update (customer_contacted) on repairs to mcp_writer;`
- Sæt `Destructive = false` og `Idempotent = true` på værktøjer som `mark_contacted`
- Lad værktøjet returnere, **hvad det ændrede**, fx "Reparation 12 (Jens Hansen, Kildemoes City): kunde kontaktet"

### 11.2 `draft_ready_sms`

Den rører ikke databasen ud over at læse. Den henter reparationen og returnerer en tekst, du selv sender. Det er et godt første skridt mod den automatiske sms i appen.

### 11.3 `add_repair`

Kræver, at prisberegningen fra `lib/repairPricing.ts` findes i C# med præcis de samme resultater (afrunding i øre, linjerabat før reparationsrabat). Skriv testene først, ud fra eksemplerne i appens tests i `Test/`.

---

## 12. Fejlfinding

| Symptom | Sandsynlig årsag | Se |
|---|---|---|
| Claude Code siger, at serveren ikke kan forbinde, eller den lukker med det samme | Noget skriver til stdout, eller `CYKELSERVICE_DB` mangler | 4.1, 9.1 |
| `Miljøvariablen CYKELSERVICE_DB mangler` | `-e` mangler i `claude mcp add`, eller `export` mangler i terminalen | 8.2, 9.1 |
| `An error occurred invoking '...'` uden detaljer | En undtagelse i din kode eller i databasen. Kør med Inspector og læs loggen på stderr. | 7.5 |
| `A parameterless default constructor or one matching signature` | Positional record og forkert type | 5.1 |
| Værktøjet svarer med en tom liste, men der er data | RLS er slået til uden policy for `mcp_readonly` | 2.2 |
| `permission denied for table ...` | `grant select` mangler for tabellen | 2.3 |
| `password authentication failed` | Forkert brugernavn på pooleren (skal være `rolle.project-ref`) | 2.4 |
| Felter er tomme eller 0 i svaret | Kolonnenavnet passer ikke med egenskaben, eller `MatchNamesWithUnderscores` mangler | 4, 2.1 |
| Claude vælger ikke dit værktøj | Beskrivelsen er for vag | 7.4 |

**Se loggen:** I Inspector vises stderr i bunden. Fra terminalen kan du køre `dotnet run 2> log.txt` og læse `log.txt` bagefter.

---

## 13. Til ansøgning og samtale

Når version 1 virker, kan du skrive noget i stil med:

> Jeg har bygget en MCP-server i C# og .NET, som giver Claude adgang til data fra mit cykelværksted. Den kører over stdio, henter data fra Postgres med Dapper og forbinder med en databasebruger, der kun kan læse.

Ting, der er gode at kunne forklare til en samtale:
- Hvorfor stdout ikke må bruges til log (4.1)
- Hvorfor serveren har sin egen databasebruger, der kun kan læse (2.3)
- Hvordan Claude vælger et værktøj ud fra beskrivelsen (7.4)
- Forskellen på `McpException` og andre fejl (7.5)
- Hvordan det minder om og adskiller sig fra function calling med Gemini i FuelPace (1.1)

## Noter undervejs

_Skriv her, hvad der drillede, og hvordan du løste det._
Tilføj mcp til claude på ny maskine:
``` bash
claude mcp add cykelservice -s user \
  -e CYKELSERVICE_DB="Host=aws-1-eu-north-1.pooler.supabase.com;Port=5432;Database=postgres;Username=mcp_readonly.sidznwefvvhckqsqfxue;Password=<kode fra password manager>;SSL Mode=Require" \
  -- ~/.local/share/cykelservice-mcp/CykelService.Mcp
```
