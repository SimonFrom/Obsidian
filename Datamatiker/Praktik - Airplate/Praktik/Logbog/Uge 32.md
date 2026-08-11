
## Sammendrag:
Ugen startede med oprydning. Jeg løste merge-konflikter fra ugen før og fjernede `group_number` fra databasen, da det viste sig overflødigt.

Tirsdag fandt jeg en central misforståelse i kravene. Jeg havde tænkt relationen som en-til-mange (scanner ↔ flere grupper), men den skulle faktisk være omvendt, en gruppe kan have flere scannere, men en scanner kan kun høre til en gruppe. Held i uheld blev det fanget tidligt via feedback, hvilket kan stå som en påmindelse om vigtigheden af ordentlig kravsspecificering, før man er langt inde i en feature.

Onsdag stødte jeg på et persisteringsproblem. Gruppenavne kunne ikke gemmes, hvis gruppen ikke havde scannere tilknyttet. Min første ide var localStorage, men den blev droppet, fordi det blandede sig med andet gemt data uden mulighed for at adskille det. Løsningen blev i stedet en dedikeret `SCANNER_GROUP_NAMES` tabel med id og navn, en renere, mere robust løsning end en client-side workaround.

Torsdag var desværre en barn syg dag.

Fredag landede min første feature på produktionsserveren, en fin milepæl. Jeg fandt og lukkede også et lille sikkerhedshul jeg selv havde skabt, gruppenavne kunne læses på tværs af organisationer, hvilket potentielt afslørede, hvor andre organisationer havde scannere placeret. Det blev løst ved at tilføje `organization_id` til gruppe-tabellen, så data isoleres pr. organisation, noget der matcher den organisation_id-isolation, jeg også har arbejdet med andre steder i systemet. Jeg fik desuden lavet en elegant løsning til gruppesletning direkte fra dropdown'en, inklusiv separat håndtering af, om scannere blot skal fjernes fra gruppen eller om selve gruppen skal slettes.

## 03-08-2026
Jeg har løst merge konflikter for min feature fra sidste uge.

Jeg har også arbejdet videre med scanner gruppe funktionen. 
Der var lidt der skulle ændres fra sidste uge database mæssigt, gruppe_nummer gav ikke mening at have med alligevel, så det skulle slettes fra docker databasen. 

## 04-08-2026
Det viste sig at jeg havde misforstået eller ikke blevet helt korrekt informeret omkring kravene til grupperings funktionen. Jeg havde forstået at der skulle være et en til mange forhold, en scanner kan være med i flere grupper. Men det viser sig at det skulle være omvendt, en gruppe kan indholde flere scannere og en scanner kan kun tilhøre en gruppe. 
Det viser bare hvor vigtigt en god start med ordentlig kravs specificering er.
Her blev det heldigvis fanget hurtigt i en tidlig feedback og er en relativt lille feature og ændring, men jo større feature eller jo længere i processen man er jo værre bliver det.

## 05-08-2026
Idag opstod et problem. Med det database design/skema jeg havde startet med at lave var der faktisk ikke nogen måde at persistere navnene på grupperne, med mindre at der var nogle scannere i gruppen hvilket der ikke nødvendigvis er. 

Jeg prøvede at lave en løsning med at bruge localStorage da min tanke var at så længe det persisterede indtil dashboardet blev lukket var fint. Men der bliver åbenbart gemt en masse andre ting i localStorage og jeg har ikke nogen måde at skille mit fra det. 
Så løsningen blev at lave en tabel mere i databasen med id og gruppe navn. Så bliver navnet gemt der og kan genbruges og forsvinder uanset om der er indhold i.


## 06-08-2026
Sygedag

## 07-08-2026
Idag kom min første feature på produktions serveren!
Det er lidt fedt at man kan se noget so man har lavet på et rigtigt produkt.
Jeg lavede også videre på grupperings featuren. Jeg havde glemt at tænke på at den måde jeg hentede på fra databasen gjorde at alle brugere kunne se alle grupper. Rent sikkersheds mæssigt er det ikke godt at alle med et login kan få en ide om hvor andre har scannere henne. Så i tabellen med gruppe navne er der tilføjet organisations id som bruges til kun at hente de grupper som man selv har oprettet.
Derudover fik jeg også styr på fjernelsen af grupper, elegant lavet i samme dropdown som oprettelsen. Funktionen fjerner også gruppen fra andre scannere hvis den slettes, ønsker man bare at fjerne den, er der en seperat funktion til det. 