## Generelt
**Indledning — hvad er det:** 
React Native er et framework, til at bygge native mobilapps til iOS og Android med JavaScript eller TypeScript og React biblioteket. 
Hele ideen med React Native er at du skriver koden én gang, og den kører på både iOS og Android. Det sparer enormt meget tid og penge sammenlignet med at bygge to separate native apps. 

**Hvad Expo tilføjer:** 
Expo er noget der ofte hænger sammen med React Native, i hvert fald når man snakker lærings og hurtige udviklings scenarier.

Det er et sæt værktøjer oven på React Native, der giver mange ting ud af kassen uden opsætning. Blandt andet hele den basale opsætning af afhængigheder, men også mere avancerede ting som hele det filbaserede navigations system. Der følger også en del moduler og komponenter med som standard.

**Afslutning:** 
Det er den hurtigste vej fra idé til en fungerende app på en telefon, hvilket er grunden til, at det er blevet det anbefalede standardvalg til nye React Native-projekter.

## Strikke appen:
Jeg har 2 produkter jeg vil fremhæve.

Det ene på grund af teknologien og det andet på grund af størrelsen

Det første er Strikkeappen, som er en simpel applikation der er lavet til at gemme og organisere ens strikkeprojekter.

Teknologien som jeg vil fremhæve er brugen af enhedens kamera. 

Igennem Expo findes der en api som gør en stor del af arbejdet for en.

Måden jeg åbner kameraet på er et simpelt check på previewUri. 
Er der en værdi, som bliver sat med form state længere oppe, skal der bare vises et preview med en image komponent. Er der ikke nogen værdi, tjekker jeg om kameraet er åbent og handler derefter. Selve kamera komponenten kommer fra BNA Components og bare droppet ind.

## Cykel-Service
Det andet projekt er min cykel-service app, som jeg vil fremhæve på grund af størrelsen og kompleksiteten. 
Hvor strikkeappen er en simpel app med ét fokus, er det her en større applikation hvor domænet er et cykelværksted.
Appen skal styre kunder, deres cykler, og de reparationer der hører til. Reparationer og reparationslinjer er kernen i domænemodellen, og det hele hænger sammen i flere niveauer: Både i strikkeappen og her har jeg brugt type komposition

En ting at fremhæve her er routing-strukturen, specielt de dynamiske ruter.
Expo Router bruger filbaseret navigation, hvor selve mappestrukturen og navngivningen definerer navigationen. 
Det lyder enkelt, men det gav mig faktisk et problem. 

Jeg havde lagt en dynamisk rute for kunder og en dynamisk rute for cykler som søskende i samme mappe — altså to ruter på samme niveau, der begge tager et id. 
Fordi jeg har brugt Expo Router understøttes dette ikke og eftersom Expo selv sætter navigationen op og forventer en bestemt struktur fejlede dette.

Løsningen var at neste ruterne i stedet for at sidestille dem. 
Cykel-ruten ligger nu inde i kunde-ruten, så stien bliver kunde-id efterfulgt af cykel-id. 
Så Expo vil automatisk bruge index filen i [id] mappen med et id som parameter og hvis der bliver givet et mere, vil [bikeId] blive brugt.
Rent logisk giver det også fint mening at man ikke kan have en cykel uden en kunde.
Det var endnu en god repræsentation af hvor vigtig struktur er i software udvikling.
