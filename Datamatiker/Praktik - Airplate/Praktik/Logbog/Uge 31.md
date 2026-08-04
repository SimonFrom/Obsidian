## Sammendrag:
Overordnet set har det været en god start på forløbet. AirPlate har taget godt imod mig og hjulpet hvor det har været nødvendigt. Kontoret er ret småt så det er nemt lige at stikke hovedet over skærmen og be om hjælp. 

Den brede viden fra undervisningen om arkitektur har hjulpet mig med at kunne finde rundt i programmet og forstå hvordan det hænger sammen og hvordan noget kommer fra server side til client side. 

Min første opgave bestod i at lave en tilføjelse til forsiden hvor der bliver vist data i form af antal scannede droner, unikke flyvninger og andre ting. Jeg skulle lave en komponent der viste en sammenligning i procent fra den matchende tidligere periode.
Jeg synes det var en god start opgave der krævede både at jeg havde fat i frontend og backend. 

Jeg ville ønske vi havde noget viden og erfaring omkring docker og brugen i praksis. Det bliver brugt en del her og virker også til at det er meget normalt ude i den virkelige verden. 


## 27-07-2026
Opstarts dag. 
Dagen gik fint, på trods at de lidt havde glemt at det var i denne uge jeg startede og ikke næste... 
Jeg fik kigget en masse dokumentation igennem og undersøgt repos. Programmet virker lige nu stort og lidt uoverskueligt, men jeg føler at når jeg kommer rigtigt igang skal det nok komme til at give mening. 

Overordnet har AirPlate været gode til at tage imod mig og Michael har hjulpet godt i løbet dagen.

## 28-07-2026
Idag fik sat nogle ekstra ting op så jeg kunne køre siden i en lokal udgave til udviklings brug. Der kører en kopi af MySQL databasen i en docker container, som siden kan trække fra.
Jeg fik også tildelt min første opgave. 
På hovedsiden er der et dashboard med antal droner fundet, flyvninger per dag og så videre. Disse kan du sortere i tidsintervaller og min opgave består i at lave et lille badge som en selvstændig komponent der viser om det viste tal er en stigning eller fald.
![[Pasted image 20260729111151.png]]

Jeg fik lavet et godt udkast idag og har fået feedback af både Michael (Software Ingeniør) og August (CEO). Michael havde ideer som jeg vil implementere imorgen.

## 29-07-2026
Jeg arbejdede videre med at rykke al UI over i en komponent og samlede alle nødvendige funktioner i en hjælper utils fil. 

Derudover rettede jeg også i forhold til feedback fra igår og gjorde hele komponenten fysisk større og mere synlig i kraft af at farven er styret af om tallet er stigende eller faldende. Ikonet blev også skiftet til et mere bastant et. Til sidst rykkede jeg det hele til siden for at bruge noget af den ellers døde plads.

Jeg er igang med at kigge på om det giver mening at flytte det fra client til server siden. Backend'en er stadig en kende uoverskuelig for mig, men det giver mere og mere mening.

Jeg fik kigget en del på backend koden idag og føler at jeg fik hul på det. Alt logik blev flyttet over på server siden og sender nu kun et tal med som bruges til procenten.

![[Pasted image 20260729110101.png]]
![[Pasted image 20260729110126.png]]

## 30-07-2026
Idag har jeg arbejdet videre på featureren fra igår. 
![[Pasted image 20260730123917.png]]

Datoen som procent er udregnet fra bliver nu sendt med fra backend og det eneste der sker client side er at dataen bliver hentet og dato formateret til lokaliseret streng.

Alle fire felter har nu også de tilsvarende tal og informationer. 

Jeg har også fået adgang til databasen og kan begynde at danne mig et overblik over den. 

Efter feedback fra Michael gik vi tilbage til dette:
![[Pasted image 20260730152128.png]]
Men nu med teksten i bunden opdatere afhængig af den specifikke valgte periode.

## 31-07-2026
Idag færdiggjorde jeg sammenlignings opgaven nok til at jeg kunne oprette min første pull request og sende den til review.
![[Pasted image 20260731151311.png]]

Der blev også opdateret så ingen ændring bliver vist på en neutral måde. 
Derudover er der også lavet automatisk oversættelse via backenden.

![[Pasted image 20260731151455.png]]

Næste opgave består i at opdatere scanner objektet og visningen af dem så det bliver muligt at oprette og opdele scannere i grupper indenfor organisationer. 
Jeg har brugt dagen på at planlægge og undersøge. 
Det vil kræve en opdatering af databasen med en ny table, scanners_group.
Jeg har skrevet sql statements og fået opdateret min lokale db i docker containeren med den nye table. 

Generelt har det været en behagelig første uge. Fra dag et har jeg haft stor frihed under ansvar og der har været hjælp at hente når jeg har bedt om det. 





