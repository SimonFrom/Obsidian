**Realtidswebapplikationer** defineres som den teknologi, der gør det muligt for brugere at modtage opdateret information uden selv at skulle anmode om data opdateringer eller trykke på en opdaterings knap, så snart data offentliggøres af udgiveren.

**SignalR** er et bibliotek, der kan bruges til at udvikle realtidswebapplikationer i .NET Core. SignalR indeholder en API, der giver server-side kode mulighed for at sende beskeder til tilsluttede klientwebbrowsere.Derudover er der god cross platform support, SDK'er kan fåes til JavaScript, C#, F#, Visual Basic og Java. SignalR gør det muligt at pushe beskeder og information direkte til klienten.

SignalR i ASP.NET Core bruger Remote Procedure Call (RPC) til at lade serveren kalde en funktion på klienten via underliggende transportprotokoller.


Realtids datatransmission er også afgørende for IoT-enheder. For eksempel skal alarmer fra IoT-enheder ved ændringer i overvågede parametre (foreksemel fra bevægelses eller temperatur sensorer) behandles og videresendes til brugere uden forsinkelse – og omvendt skal kommandoer fra brugerne nå enhederne øjeblikkeligt.

I webudvikling skelner man mellem to grundlæggende dataoverførselsmetoder: pull og push teknologier.

**Pull** teknologien er den traditionelle metode, hvor browseren aktivt anmoder om information fra serveren. Hver gang du loader en hjemmeside eller klikker på et link, sender din browser en forespørgsel, og serveren sender de ønskede data tilbage. Det er som at gå hen og banke på døren for at få fat i noget information.

**Push** teknologien er mere dynamisk. Her sender serveren aktivt data til browseren, uden at der er behov for en specifik anmodning. Det minder om at få en besked med det samme, næsten som en sms. Denne teknologi bruges i realtidsapplikationer som chat, livestreaming, sociale mediers notifikationer og online spil, hvor øjeblikkelig information er afgørende.

**Pull** styres af klienten og **Push** styres af serveren.

### Samspillet mellem SignalR og Blazor afhænger af Blazors hostingmodel:
1.


