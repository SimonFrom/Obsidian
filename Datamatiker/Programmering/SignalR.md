**Realtidswebapplikationer** defineres som den teknologi, der gør det muligt for brugere at modtage opdateret information uden selv at skulle anmode om data opdateringer eller trykke på en opdaterings knap, så snart data offentliggøres af udgiveren.

**SignalR** er et bibliotek, der kan bruges til at udvikle realtidswebapplikationer i .NET Core. SignalR indeholder en API, der giver server-side kode mulighed for at sende beskeder til tilsluttede klientwebbrowsere.Derudover er der god cross platform support, SDK'er kan fåes til JavaScript, C#, F#, Visual Basic og Java. SignalR gør det muligt at pushe beskeder og information direkte til klienten.

SignalR i ASP.NET Core bruger Remote Procedure Call (RPC) til at lade serveren kalde en funktion på klienten via underliggende transportprotokoller.

Realtids datatransmission er også afgørende for IoT-enheder. For eksempel skal alarmer fra IoT-enheder ved ændringer i overvågede parametre (foreksemel fra bevægelses eller temperatur sensorer) behandles og videresendes til brugere uden forsinkelse – og omvendt skal kommandoer fra brugerne nå enhederne øjeblikkeligt.

I webudvikling skelner man mellem to grundlæggende dataoverførselsmetoder: pull og push teknologier.

**Pull** teknologien er den traditionelle metode, hvor browseren aktivt anmoder om information fra serveren. Hver gang du loader en hjemmeside eller klikker på et link, sender din browser en forespørgsel, og serveren sender de ønskede data tilbage. Det er som at gå hen og banke på døren for at få fat i noget information.

**Push** teknologien er mere dynamisk. Her sender serveren aktivt data til browseren, uden at der er behov for en specifik anmodning. Det minder om at få en besked med det samme, næsten som en sms. Denne teknologi bruges i realtidsapplikationer som chat, livestreaming, sociale mediers notifikationer og online spil, hvor øjeblikkelig information er afgørende.

**Pull** styres af klienten og **Push** styres af serveren.

### Brugere:
I SignalR kan brugere identificeres og håndteres på flere måder:
**Connection ID**
- Hver bruger får automatisk tildelt et unikt Connection ID, når de opretter forbindelse    
- Dette ID kan bruges til at sende beskeder til specifikke brugere
- Connection ID'et ændres dog, hvis brugeren genopretter forbindelsen
    
**User Identity**
- Du kan knytte autentificerede brugere til deres [ASP.NET(opens in a new tab)](http://ASP.NET) Identity
- Dette giver mulighed for at spore brugere på tværs af forbindelser
- Brug Context.User.Identity.Name for at få brugernavnet

**Persistente forbindelser**
- SignalR tilbyder mulighed for at gemme brugerinformation
- Dette kan bruges til at genoptage sessioner ved genoprettelse af forbindelser

### Samspillet mellem SignalR og Blazor afhænger af Blazors hostingmodel:
- **Blazor Server**:
	Her er SignalR **kritisk infrastruktur**.Applikationen kører på serveren, og alle UI-opdateringer sendes til klienten via en SignalR-forbindelse. Dette muliggør øjeblikkelige opdateringer uden page reloads. Eksempel: En live dashboard-app, der viser aktuelle data.
- **Blazor WebAssembly:**
	SignalR er **valgfri, men kraftfuld**. Frontenden kører i browseren, men kan forbinde til en SignalR-hub på serveren for realtidsfunktioner som chat eller notifikationer. Eksempel: En aktiekurs-app, der streamer ændringer i realtid.


#### Nøglepunkter:
- **C#-integration**: Begge teknologier bruger C#, hvilket undgår behov for JavaScript i mange situationer.
- **HubConnectionBuilder**: Blazor-komponenter kan direkte oprette SignalR-forbindelser via C#-kode.
- **Realtidssamarbejde**: Ideel til apps, der kræver synkronisering mellem brugere (f.eks. kollaborativ kommunikation).


#### Implementeringen af SignalR i et projekt optræder trinvist:

- Først oprettes hub-klassen på serveren
    
- Derefter konfigureres SignalR i startup-koden,
    
- Klientbiblioteket integreres i webapplikationen,
    
- Forbindelsen til hubben etableres,
- 
- Klientmetoder defineres til serveranmodninger, og endelig får klienten mulighed for at kalde servermetoder.

#### The hub:
1. **Abstraherer kompleksitet**:  
    Hubs anvender; **RPC (Remote Procedure Calls)**, således at du kan kalde metoder direkte på klienter fra serveren (og omvendt) uden at håndtere low-level kommunikation som WebSocket eller HTTP-direkte.
    
2. **Håndterer forbindelser**:  
    Automatisk sporing af tilsluttede klienter, grupper og livscyklus-hændelser (f.eks. når en klient forbinder/fra-kobler).
    
3. **Broadcast og grupper**:  
    Send beskeder til alle klienter, specifikke klienter eller grupper (f.eks. et chatrum).