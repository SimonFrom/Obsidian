## Mål:
- Output i form af kurve der viser et bud på hvornår de forskellige typer af events kan forvente at sælge og i hvilket omfang.
- Et bud på om man kan forvente at eventet vil blive udsolgt.

### Krav:
- Events og arrangører skal have angivet typer. Disse behøver ikke være 100% korrekte i denne omgang, men skal være konsistente og konsekvente.
- Der skal også sorteres nogle arrangører fra, Langesø MTB f.eks da de sælger billetter per behov og ikke per event. Disse arrangører skal identificeres og behandles.


### Klasse:
Event:

- `start` — eventdato
- `sellStart` / `sellEnd` — definerer salgets længde
- `maxAmount` — samlet kapacitet, nødvendig for at beregne salgsprocenten
- `tickets[].amount` — tilgængeligt lager per billettype
- `tickets[].price` — prisniveau
- `locationCity` / `locationPostal` — lokation placering
- `categories[]` — siddezoner og deres kapaciteter
- `seating.active` — om det er et event med pladsnummerering
- `eventType` — ENKELT vs tilbagevendende
- `createdAt` — hvornår eventet blev oprettet (tid fra oprettelse til salgsstart)
- `showCountdown` — signalerer at urgency-markedsføring er aktiv

Transaktion:

- `createdAt` — præcist købstidspunkt, central feature til opbygning af hastighedskurver
- `tickets[].amount` — billetter per ordre
- `realPrice` — faktisk omsætning per ordre
- `status` — kun FULDFØRTE ordrer bør anvendes
- `salesChannelId` — web vs POS vs intern, påvirker salgsmønsteret
- `ipLookup.lookup.city` / `.region` — købers oprindelse til geografiske features
- `appliedCoupons` — om rabatter påvirkede købstidspunktet
- `appliedDiscountGroups` — samme

Afledte features du ville beregne:

- Dage fra salgsstart til køb (købstiming)
- Dage fra køb til eventstart (urgency-signal)
- Salgshastighed ved dagsintervaller (f.eks. solgte billetter per dag)
- Kumulativ salgsprocent på hvert tidspunkt
- Salgets længde i dage
- Kapacitetsudnyttelsesrate