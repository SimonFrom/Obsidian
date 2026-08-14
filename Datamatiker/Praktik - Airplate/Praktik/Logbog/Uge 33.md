
## Sammendrag:
Ugen startede med at færdiggøre de sidste dele af grupperings-featuren, som blev sendt til review. Fordi der havde været en klar beskrivelse fra start og løbende feedback undervejs, var der kun få rettelser tilbage. Jeg oprettede pull requests på både frontend og backend, samt et database-migrationsscript til at opdatere dev-databasen. Da featuren efterfølgende skulle merges ind til dev-testing, opstod der en del merge-konflikter, som umiddelbart virkede uforklarlige givet featurens korte levetid. Den sandsynlige forklaring var, at branchen oprindeligt var lavet ud fra en tidligere feature-branch frem for master, hvilket naturligt gav en del divergens. Konflikterne blev løst med en merge commit, hvorefter alt kunne merges korrekt – en nyttig påmindelse om vigtigheden af god git struktur og disciplin. Der var også mindre UI justeringer undervejs, primært farveændringer for et andet visuelt udtryk.

Herefter gik jeg i gang med en ny opgave, som jeg forsøger at gribe struktureret an: først planlægning og undersøgelse af eksisterende kode, hvilket stadig kan tage tid i en så stor kodebase, dernæst selve udførslen, hvor især TypeScript-erfaring til tider er en udfordring, hvorfor jeg har fået lavet nogle øvelser til selvstudie; og til sidst test og review, som foregår ad to omgange. 
Løbende peer reviews samt en afsluttende brugertest på dev serveren med resten af virksomheden. Den konkrete opgave handler om at kunne til og fravælge notifikationer på sensor og zoneniveau. Sensordata er allerede tilgængelig på frontend, men zonedata ligger kun på backend, hvilket kræver en større og velplanlagt backend ændring. Ugen sluttede med bug fixing og refaktorering af en filterfunktion til zonevalg, som både berører frontend og backend, og hvor backend-løsningen også skal genbruges til sensor notifikationerne. Da opgaven kunne løses på flere måder, brugte jeg en tid på at sparre med kolleger og Claude for at få overblik over den bedste tilgang som jeg vil implementere i næste uge.

Sideløbende besluttede studiegruppen at indgå samarbejde med Frank Institute of Sports om en webapp til beregning af kalorie- og kulhydratindtag ved cykling, løb og triathlon til brug i deres coaching, en proces der i dag foregår manuelt i Excel.

## 10-08-2026
Der var en lille smule arbejde tilbage på grupperings featuren inden jeg sendte den første review. 
Der var ikke de store rettelser, både på grund af en beskrivelse fra start af, men også løbende feedback. 
Jeg lavede pull request på både front end og backend siden og skrev et database migrations script til nemt at kunne opdatere dev databasen.

## 11-08-2026
Påmindelse om git struktur og disciplin. Da grupperings featureren skulle merges ind til dev testing var der en masse merge konflikter. Hvilket jeg ikke helt kunne forstå, eftersom at det ikke var så lang tid siden den var oprettet. Men mit umiddelbare gæt at jeg har branchet ud fra min tidligere feature branch og ikke master. Så der var jo helt naturligt en masse ting der manglede. De blev løst med en merge commit og derefter kunne alt merges. 
Der kom også lidt små ændringer til UI, mest nogle farver der lige skulle ændres lidt for et andet visuelt udtryk.

## 12-08-2026
Dagen gik med opstart på en ny opgave. Ved nye opgaver har jeg prøvet at strukturere det på følgende måde:
- Planlægning og undersøgelse af nuværende kode:
	- Programmet er stadig meget stort og omfattende for mig, så bare øvelsen med hvor pokker koden skal skrives henne kan nogle gange godt tage lidt tid. 
	  Dernæst skal jeg så også gerne forstå det kode der er i forvejen og som den nye kode måske skal snakke sammen med.
- Udførsel:
	- Hvis fase 1 er gået godt er dette næsten den nemme del. Det har dog varieret meget føler jeg. Om det er på grund af Typescript , som jo trods alt stadig er meget nyt for mig eller manglende erfaring skal jeg ikke kunne sige.
	  Umiddelbart mener jeg at jeg har en god forståelse for arkitekturen og nødvendigheden for den, så jeg tænker simpelthen at det er manglende erfaring. Jeg har også på den baggrund fået Claude til at lave nogle Typescript øvelser til mig som jeg kan bruge til at øve med.
- Test/Review:
	- Her har jeg også brugt Claude her i starten. Min plan er at det er noget af det næste jeg gerne vil kigge på i mine "selv lavede" kurser. Jeg snuste lidt til det under valgfags perioden, men ikke nok til at jeg kan skrive noget der er grundigt nok.
	- Review kører af to omgange. Et par hurtige peer reviews igennem iterationerne af opgaven og tilsidst en "bruger test" på vores dev server som resten af virksomheden hjælper med.

## 13-08-2026
Idag har været lidt en planlægning og tænke tanker dag. 
Den nye opgave er en mulighed for at vælge notifikationer til eller fra på sensorer eller områder. 
Sensorer fungerer umiddelbart som det skal da alt data man skal bruge er tilgængelig på frontend siden, men zonerne er kun tilgængelige på backend siden. 
Så det kræver en lidt større ændring backend mæssigt som lige skal planlægges ordentligt. 

Derudover havde vi i studie gruppen også planlagt et møde til at drøfte mulige afsluttende samarbejdspartnere og projekter. Vi blev enige om at sige ja tak til et samarbejde med Frank Institute Of Sports omkring at lave en webapp til at beregne kalorie og kulhydrat indtag i forbindelse med sport, nærmere bestemt cykling, løb og triathlon som kan indgå i deres coaching af kunder. Det er en proces som idag bliver gjort manuelt igennem excel ark, så en oplagt mulighed for at sætte lidt mere strøm til et projekt. 

## 14-08-2026
Dagen idag stod på lidt bug fixing og refaktorering af en filter funktion til at vælge zoner til og fra. Det er både frontend og backend der skal ændres og ændringen på backend skal også bruges løsningen med at kunne vælge sensorer fra eller til.

Det bliver en lidt større ændring og der er forskellige måde det kunne gøres på, så jeg har brugt noget af dagen på at sparre med kollegaer og Claude for at danne et overblik. 