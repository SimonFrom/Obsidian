## Generelt
**Indledning — hvad er det:** 
React Native er et framework, til at bygge native mobilapps til iOS og Android med JavaScript eller TypeScript og React biblioteket. 
Hele ideen med React Native er at du skriver koden én gang, og den kører på både iOS og Android. Det sparer enormt meget tid og penge sammenlignet med at bygge to separate native apps. 

**Hvad Expo tilføjer:** 
Expo er noget der ofte hænger sammen med React Native, i hvert fald lærings og hurtig udviklings scenarier.

Det er et sæt værktøjer oven på React Native, der gør mange ting for en ud af kassen. Blandt andet hele den basale opsætning, men også mere avancerede ting som hele det filbaserede navigations system f.eks.

Expo fjerner det meste af den besværlige opsætning. Du behøver ikke have Xcode eller Android Studio konfigureret for at komme i gang, i stedet er der Expo Go — en app, der lader dig se dit projekt med det samme på en rigtig telefon. Der følger et stort bibliotek af færdige native moduler (kamera, lokation, notifikationer).

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
Live gif der kører i baggrunden og snak noget om hvorfor og state management