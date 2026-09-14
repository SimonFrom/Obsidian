## Sammendrag
Denne uge har jeg tænkt en del over, hvordan man kunne gøre undervisningen bedre – jeg tror det ville hjælpe med øvelser i at læse og finde rundt i større systemer end dem vi laver i undervisningen, gerne rigtig produktionskode der er lidt rodet og har teknisk gæld hist og her. Det har været en af de ting jeg har fundet mest demotiverende de første uger i praktikken. Kunne evt. være et forløb over en uge eller to, hvor man får udleveret udokumenteret kode og selv skal dokumentere den og bygge nyt oven på, der spiller sammen med det gamle.

Jeg har også opdaget noget om mig selv: jeg er god til at tage imod opgaver og gå i gang, men jeg mister overblikket undervejs, og så falder både kvaliteten af arbejdet og mit udbytte af det. Det er ikke noget jeg har skullet forholde mig til før på studiet, for mængden af opgaver har været meget mere overskuelig de første fire semestre.

Rent fagligt har jeg brugt tiden på en feature, hvor man skal kunne se historisk aktivitet, når man opretter en ny alarmzone. Lyder simpelt, men er det ikke helt – der kan være tusindvis af GPS-spor der skal tjekkes, og en naiv gennemgang ville nemt tage 15-30 minutter. Løsningen bliver at tjekke take off- eller operatørlokation op imod 10-15 km fra zonen først, da krydsninger længere væk er usandsynlige – det sparer en masse tid. Jeg arbejder også på at flytte selve databasegennemgangen over i en baggrundstråd, så man kun ser et lille udsnit af data lige efter zonen er oprettet, og har to lignende men forskellige ideer jeg går videre med.

## 07/09/2026
Et forslag som du kan tage med til de andre undervisere og måske endda bruge til de kommende hold, kunne være nogle øvelser i det her med at læse og finde rundt i noget større systemer end dem vi laver i undervisningen. Gerne noget rigtig produktions kode som er dejligt rodet og med teknisk gæld hist og her. 
Det er noget som jeg synes har været enormt de-motiverende i de første par uger. 
Hvordan man lige gør det praktisk i forbindelse med undervisningen, har jeg ikke lige den gode ide til. 
Men det kunne sagtens være et lidt længere forløb over en uge eller to hvor de studerende fik udleveret koden uden noget dokumentation og så skulle dokumentere og eventuelt implementere nye tbng i programmet som snakker sammen med det gamle. 

## 08/09/2026
En ting jeg skal arbejde med personligt, er det med at tage munden for fuld. 
Jeg er rigtigt god til bare at tage imod arbejde og gå igang med det, men jeg kan godt mærke at jeg mister overblikket og derved falder kvaliteten af arbejdet og min læring. 
Det er ikke noget som jeg føler jeg har skulle tænke over på studiet før, men mængden af opgaver og ting man skulle på de første 4 semestre har også været meget begrænset. 

## 09/09/2026


## 10/09/2026
Jeg er igang med at lave en ny feature som har vist sig at være ret omfattende og med ret meget kød på. Opgaven går ud på at når man laver en ny alarm zone, skal den historiske aktivitet kunne ses. 
I teorien er det jo simpelt nok, men der er mange faktorer der spiller ind. 
En er mængden af data der kan være. Det kan nemt flere tusinder gps spor med enkelte punkter der er gemt i databasen, der skal tjekkes igennem for om de koordinater rammer noget i den nye zone. Med de linjer der er i databasen, er det ikke utænkeligt at det ville tage et sted imellem 15-30 minutter at kigge det hele igennem for at bedømme om der er krydset ind i alarmzonen. Løsningen bliver højst sandsynligt at starte med at tjekke enten take off lokation eller operatør lokation og se om de er indenfor 10-15 km af den nye zone, det er meget usandsynligt at der vil være nogle krydsninger af zonen så langt væk fra og det er meget hurtigere at tjekke en lille del af hver linje end at skulle gå hele gps sporet igennem. 
## 11/09/2026
En anden ting til zone backfill som jeg arbejder på er at flytte den store opgave i at lede hele databasen igennem til en baggrunds tråd og så kun vise et lille udsnit lige efter zonen er lavet. 
Jeg har to ideer som jeg vil arbejde videre udfra, de minder meget om hinanden men er ret forskellige alligevel.
