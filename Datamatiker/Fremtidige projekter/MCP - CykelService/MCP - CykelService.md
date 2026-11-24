---
tags: [projekt, mcp, csharp, cykelservice]
status: idé
oprettet: 2026-11-24
---

# MCP - CykelService

En MCP-server i C# og .NET, der giver Claude adgang til data fra min cykelservice-app (kunder, cykler og reparationer i Supabase). Så kan jeg spørge om ting, jeg ellers skal klikke mig frem til i appen.

Eksempler på spørgsmål, den skal kunne svare på:
- "Hvad har jeg lavet på Jens' cykel før?"
- "Hvilke cykler er færdige, men kunden er ikke kontaktet endnu?"
- "Hvor meget har jeg tjent i november?"
- "Hvem mangler at betale?"

## Hvorfor

- Jeg bruger den selv, så den bliver ikke glemt efter en uge.
- Den er skrevet i C# og .NET, som er det, Hesehus og mange andre bruger.
- Jeg har kun brugt MCP som bruger (Google Docs og Drev). Med det her projekt kan jeg skrive, at jeg selv har bygget en MCP-server.
- Den passer med noget, jeg allerede har planlagt i appen: en sms, når cyklen er klar.

**Tidsplan:** Ansøgningsfristen hos Hesehus er 14. januar 2027, og eksamen er i uge 4. Målet er, at version 1 (kun læsning) virker før jul.

## Hvad er MCP, kort fortalt

- **MCP (Model Context Protocol)** er en standard for at give en AI-model adgang til værktøjer og data.
- **Serveren** (det, jeg bygger) beskriver nogle **værktøjer** med navn, beskrivelse og parametre.
- **Klienten** (Claude Code eller Claude Desktop) starter serveren og viser værktøjerne til modellen. Modellen vælger selv, hvornår et værktøj skal kaldes.
- **Transport:** Lokalt kører det over **stdio**, så klienten starter programmet og taler med det gennem stdin og stdout. Det er det nemmeste at starte med.
- Det minder om function calling med Gemini i FuelPace, men værktøjerne ligger i en separat server, som alle MCP-klienter kan bruge.

> [!warning] Stdout er reserveret til protokollen
> Over stdio må programmet ikke skrive noget til stdout, heller ikke log-beskeder eller `Console.WriteLine`. Ellers går forbindelsen i stykker. Log til stderr.

## Data i Supabase

Tabellerne er aflæst fra `~/Code/cykel-service-v0` (`crud/` og `types/`). Tjek de præcise kolonnenavne i Supabase, før koden skrives.

| Tabel | Kolonner |
|---|---|
| `customers` | `id`, `first_name`, `last_name`, `street_name`, `street_number`, `postal_code`, `city`, `email`, `phone_number`, `club`, `note` |
| `bikes` | `id`, `customer_id`, `make`, `model`, `colour`, `type` (mtb, city, shoppping, kid, racer, electric) |
| `repairs` | `id`, `bike_id`, `customer_id`, `total_price`, `discount_type`, `discount_value`, `note`, `created_at`, `intake_date`, `delivery_date`, `complete`, `payment_recieved`, `customer_contacted`, `missing_items` |
| `repair_items` | `id`, `repair_id`, `title`, `price`, `quantity`, `total`, `discount_type`, `discount_value` |

Ting at være opmærksom på:
- Kolonnen hedder `payment_recieved` (med stavefejl). Det skal også stå sådan i SQL'en.
- Cykeltypen `shoppping` har tre p'er i appen.
- `repairs.total_price` er gemt i databasen efter rabatter. Læs den i stedet for at regne prisen ud igen. Prisberegningen ligger i `lib/repairPricing.ts`, og den skal ikke kopieres til C#, før jeg laver `add_repair`.

## Værktøjer

### Version 1: kun læsning

| Værktøj | Parametre | Hvad det gør |
|---|---|---|
| `find_customer` | `query` (navn eller telefon) | Finder kunder og viser deres cykler |
| `bike_history` | `customerId` eller `bikeId` | Viser alle reparationer med linjer og priser |
| `open_repairs` | ingen | Viser reparationer, der ikke er afsluttede, med flag for mangler, kontakt og betaling |
| `unpaid_repairs` | ingen | Viser afsluttede reparationer, hvor `payment_recieved` er falsk |
| `revenue` | `from`, `to` | Summerer `total_price` for perioden, opdelt på måned |

### Version 2: skrivning (først når version 1 virker)

| Værktøj | Hvad det gør |
|---|---|
| `mark_contacted` | Sætter `customer_contacted` på en reparation |
| `draft_ready_sms` | Skriver en "din cykel er klar"-besked ud fra reparationen. Den sender ikke noget. |
| `add_repair` | Opretter en reparation. Kræver, at prisberegningen fra `repairPricing.ts` er lavet i C# med de samme tests. |

Skrivende værktøjer skal vise, hvad de vil ændre, og Claude Code beder om lov, før de køres. Lad være med at lave et værktøj, der sletter noget.

## Teknik

- **.NET** med den officielle C#-SDK til MCP, NuGet-pakken `ModelContextProtocol`. Tjek den nyeste version og README'en på GitHub (`modelcontextprotocol/csharp-sdk`), før jeg starter, da API'et har ændret sig undervejs.
- **Database:** Forbind direkte til Supabase' Postgres med **Npgsql** (eventuelt med Dapper). Det er enklere end at kalde Supabase' REST-API fra C#.
- **Opbygning:** Brug samme lagdeling som i FuelPace, men i lille skala:
  - `Tools/`: MCP-værktøjerne, der kun tager imod parametre og formaterer svaret
  - `Data/`: forespørgsler mod databasen, bag et interface, så de kan testes
  - `Models/`: records for kunde, cykel og reparation

Skitse af, hvordan en server og et værktøj ser ud med SDK'en. Tjek den mod README'en, før den bruges.

```csharp
var builder = Host.CreateApplicationBuilder(args);
builder.Logging.AddConsole(o => o.LogToStandardErrorThreshold = LogLevel.Trace);

builder.Services
    .AddMcpServer()
    .WithStdioServerTransport()
    .WithToolsFromAssembly();

await builder.Build().RunAsync();

[McpServerToolType]
public class RepairTools(IRepairQueries queries)
{
    [McpServerTool, Description("Reparationer, der ikke er afsluttede endnu.")]
    public Task<IReadOnlyList<OpenRepair>> OpenRepairs() => queries.GetOpenRepairsAsync();
}
```

## Sikkerhed

- **Adgangskoden til databasen** ligger i en miljøvariabel eller i `dotnet user-secrets`, aldrig i git. Tilføj `.gitignore` fra start.
- **Database-bruger kun til læsning:** Opret en Postgres-rolle til version 1, der kun har `SELECT` på de fire tabeller. Så kan serveren ikke ændre data ved en fejl. Forbindelsen direkte til Postgres går uden om appens login og RLS, så rollen er den eneste beskyttelse.
- **Persondata:** Kundernes navne, telefonnumre og adresser bliver sendt til Claude, når et værktøj svarer. Send kun de felter, der er brug for til svaret.

## Plan

- [ ] Tjek kolonnenavne og datatyper i Supabase (tabeleditoren eller `information_schema.columns`)
- [ ] Opret en Postgres-rolle, der kun kan læse, og find connection string under Project Settings → Database
- [ ] `dotnet new console`, tilføj `ModelContextProtocol`, `Microsoft.Extensions.Hosting` og `Npgsql`
- [ ] Lav ét værktøj (`open_repairs`) og få det til at virke hele vejen
- [ ] Test med MCP Inspector: `npx @modelcontextprotocol/inspector dotnet run --project <sti>`
- [ ] Tilføj til Claude Code: `claude mcp add cykelservice -- dotnet run --project <sti>`
- [ ] Lav resten af værktøjerne fra version 1
- [ ] Skriv tests af forespørgslerne med testdata
- [ ] README med opsætning, og læg projektet på GitHub uden adgangskoder
- [ ] Brug den i en uge på værkstedet og skriv ned, hvad jeg savner
- [ ] Version 2: `mark_contacted` og `draft_ready_sms`

## Noter undervejs

_Hvad virkede, hvad drillede, og hvad ville jeg gøre anderledes. Det er godt at have til en jobsamtale._

## Links

- Appen: `~/Code/cykel-service-v0`
- MCP-dokumentation: https://modelcontextprotocol.io
- C#-SDK: https://github.com/modelcontextprotocol/csharp-sdk
- [[Clean Architecture]]
