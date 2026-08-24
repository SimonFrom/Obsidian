## Sammendrag:


## 17-08-2026
Sløv dag.
Det har faktisk været en træls dag at komme igennem idag. Jeg er blevet enormt meget i tvivl om jeg egentlig har valgt den rigtige uddannelse. 
I den her sammenhæng tænker jeg at det mest er fordi jeg har svært ved rigtigt at "føle" noget for det projekt jeg er i lige nu, i og med at det ikke er "mit" og jeg har ikke har været med fra start, hvilket jo kan give nogle udfordringer når jeg skal ud og arbejde rigtigt. Jeg tænker ærligt talt mere over vores kommende eksamens projekt og glæder mig mere til at komme igang med det, end de næste 7 uger her.

Men jeg holder ved og fortsætter selvfølgelig. 

## 18-08-2026
Notifikations feature klar til review. Jeg er lidt i tvivl om det rent faktisk virker efter hensigten da den websocket der bruges til kommunikation ikke er tilgængelig når programmet køres som localhost grundet nogle certifikater som bliver sat op når der er kontakt til serveren. 
Dog har jeg testet med manuelt lavet objekter som bliver sendt til frontend, som skulle ligne det backenden burde sende. 

## 19-08-2026
Det meste af dagen gik med flytning af kontor.
Men jeg fik startet op på at skrive Playwright test til notifikations features. 
Mønstret minder meget om AAA som vi kender fra vores unit tests i undervisningen, men med en anden syntax selvfølgelig. Men overordnet med arrange, act og assert er det meget ens.

## 20-08-2026
Jeg skrev videre på tests. 
Eftersom at man nogle gange programmere at "testen" skal klikke på en knap eller valgmulighed, kan man være nødt til lige frem at programmere en ventetid eller dobbelt tjekke at det rent faktisk eksisterer på siden inden der klikkes, ellers vil testen fejle på det og ikke på om koden rent faktisk fungerer efter hensigten.

Motivationen er også vendt igen efter starten på ugen, hvorfor ved jeg ikke, men jeg er mere positivt stemt nu i hvert fald.

## 21-08-2026
Jeg fik skrevet testene færdige. Jeg valgte at opdele det i 3 forskellige test for at have en sund afkobling mellem dem.
Nu skal jeg igang med at dokumentere de nye features jeg har lavet indtil så de kan sendes ud til kunder og komme rigtigt i produktion.


### Svar til Leif på mail:
Virksomhedens holdning til AI er rimelig løs. 

Noget med relatering til skolen og læringen

En ting jeg har brugt meget har været i starten simpelthen at få Claude til at vise mig vejen igennem programmet fra frontend til backend, noget som jeg godt kan synes var enormt svært i starten. Der er ikke helt samme stramme arkitektur som vi har lært i undervisningen. På den måde har jeg så kunne efterfølgende meget nemmere sige at "den variabel skal ændres inden den rammer frontenden og det er lige her", istedet for først at skulle lede/søge mange filer og linjer igennem for en variabel eller funktion, som jeg måske ikke havde navnet på. 

Min egen holdning til brugen af AI i det her fag har altid været at jeg bruger det som en avanceret form for Google. Støder jeg på et problem jeg ikke lige umiddelbart kan løse, sender jeg det videre med kontekst og informationen og beder ikke om en direkte løsning, men guiding eller ideer som jeg kan udforske selv, og udtrykkeligt om at jeg skal have forklaret hvordan og hvorfor en eventuel løsning virker. 

Virksomheden har en delt Claude Max 5x plan som vi er 5 udviklere der deler. Jeg har ikke fået en direkte grænse at vide, men har heller ikke spurgt. Jeg bruger heller ikke credits til det jeg har brugt den til indtil videre, så det har ikke været relevant. 




