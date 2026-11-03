
#### Trænings makker afstand:
[Zip fil med DK postnumre](https://download.geonames.org/export/zip/)
Statisk fil som lever i programmet og gør at vi kan få deterministiske opslag med afstand hver gang.
Hvert postnummer får et ca center punkt som man med en formel kan beregne afstanden imellem. 
Ingen HTTP kald, ingen database lookup. Ren matematisk kode. 
Giver os også en måde at validere postnumre ved indtastning, på anden måde kun at tillade tal.
#### Lokation:
[Open Meteo GeoCoding](https://open-meteo.com/en/docs/geocoding-api)
Kan lave lokations navne om til koordinater som Forecast api'et skal bruge.
Kræver at vi giver credit til dem i programmet på den gratis licens.
#### Vejr:
[Open Meteo Forecast](https://open-meteo.com/en/docs)
Tilbyder op til 14 dage forud og umiddelbart har de droppet den begrænsning vi ramte i tidligere projekt med længde på vejrudsigt
En løsning på at kunne regne langt forud kunne være at bruge data fra sidste år. 
En hurtig test med Odense idag og et år tilbage viser at der er en forskel på 0,1 grad.
Det vil give et godt bud på en foreløbig plan og når man så er tættere på vil man kunne lave en mere præcis.
Kræver at vi giver credit til dem i programmet på den gratis licens.
#### Gemini:
Kan åbenbart ikke det vi havde håbet. Vi vil ikke kunne få json retur, kun løs tekst som vi ikke har en chance for at sortere ordentligt i. 
Den vil nok godt kunne returnere en tekst med hvad vejret forventes at blive, men vi kan ikke tjekke kilder eller verificere at det rent faktisk passer overens med nogle vejrudsigter. 
