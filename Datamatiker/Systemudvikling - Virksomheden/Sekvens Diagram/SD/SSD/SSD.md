SSD er et artefakt der illustrerer input og output begivenheder (events), relateret til systemet der diskuteres.
 
Det er vigtigt at man kun laver SSD til 1 valgt use-case og ikke dem alle medmindre der er sagt andet.  
Et eksempel på en SSD for et salg ville se således ud.
   
![Et billede der indeholder tekst kvittering diagram...](Exported%20image%2020251104231252-0.png)  

Ligesom en ekstern aktør bruger et system i en use-case, så viser SSD mere konkret hvad der sker i systemet undervejs i scenariet f.eks. en salgsperson spørger POS system om at registrere et køb via et produkts ID i en use-case, det skrives i SSD som ”the enterItem event”.  
Så en SSD beskriver et enkelt use-case scenarie, hvilke handlinger den eksterne aktør genererer, deres rækkefølge og hvilke handlinger der sker inde i systemet. Den behandles lige som alle andre modeller, som en black box.
   
![Et billede der indeholder tekst skrmbillede diagra...](Exported%20image%2020251104231254-1.png)  

Ligesom med use-cases så er det bedst at være abstrakt i navngivningen, et eksempel kunne være ved kød af et produkt, se 10.4.  
Scan(itemID) er dårlig navngivning da det insinuerer at der bliver scannet noget, og ved køb vil man ikke nødvendigvis scanne et produkt.  
enterItem(itemID) er her et bedre valg, da i (næsten) alle systemer bliver der gjort brug af det samt det er abstrakt. (andre gode ord at bruge ville være add, enter, end, make osv.)
   
![Et billede der indeholder tekst skrmbillede Fontsk...](Exported%20image%2020251104231255-2.png)  

Billedet viser systemhændelser, systemoperationer, systemrespons og operationskontrakt.
   
![Et billede der indeholder tekst skrmbillede nummer...](Exported%20image%2020251104231256-3.png)  

Der skal ikke laves SD for alle SSD, billedet viser bare hvordan der er sporbarhed mellem de 2 forskellige modeller.
   
![Exported image](Exported%20image%2020251104231258-4.png)  

Vores SSD fra Disaheim ”Salg af produkt”
 
# SSD kvalitetskriterier

- Navngivningen skal være ens på tværs af modellerne.
- Der skal være en overskrift på som viser hvilken use case diagrammet viser.
- Skal være lavet over en use case.
- Systemhændelse skal være tegnet som en fuldt optrukken linje med en udfyldt pil.
- Systemrespons skal være en stiplet linje med ikke udfyldt pil.
- Aktør skal være til venstre og systemet til højre.
- Beskrivelser skal være på virksomhedens sprog.
- Retur beskeder skal ikke have parenteser.