[Sekvens Diagrammer](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-sequence-diagram/)  
1:

- Med et SD kan man danne sig et over blik om man har alle de operationer i sit Software design diagram man har brug for i sit program.
- Det er også muligt at få et konkret overblik over hvad der starter hvad og returner hvad.
- Man kan også se hvilke klasser eller objekter der er afhængige af hinanden.
- Diagrammet kan også kobles sammen med Use Cases, de skal gerne afspejle main succes scenario.

2:

- LifeLine - Indivuel
- Aktiviterings søjle - Indikere om et objekt er aktivt og i hvor lang tid
 
3:

- Message -
- Den stiplede linje – Retur meddelelse omkring hvilken type der bliver sendt tilbage.
    - Mod venstre er retur.
    - Mod højre er et call mod et objekt der skal aktiveres.
 
4:
   

-   
    
![Exported image](Exported%20image%2020251104231334-0.png)   
## Øvelse 2

![Menu Title string iternCount int O constructcr str...](Exported%20image%2020251104231335-1.png) ![Prog ram Main for each me u bop itemd O I itemi li...](Exported%20image%2020251104231336-2.png)

## Øvelse 2.2 Find Sammenhænge
 - Navngivningen er ens.
- Multipliciteten, Menu fremgår engang hvorimod MenuIten fremgår flere gange.
- Begge diagrammer viser hvilke klasser og objekter der indgår.
   

## Øvelse 2

## Øvelse 2.2 Find Sammenhænge
   

## Øvelse 2.3: Opstil kvalitetskriterier

## Øvelse 2.3: Opstil kvalitetskriterier

- Navngivningen skal være ens.
- Er der noget overflødigt i Design modellen, så fjern det.
- Mangler der noget i Design modellen , så få det tilføjet. 
## Øvelse 3.1: Lav pseudokode for Main()-beskeden i Program-klassen
   

SelectMenuItem starter og gemmer en int i itemId
 
## Øvelse 3.2: Lav pseudokode for Show()-beskeden i Menu-objektet
 
/*Bool show = "skal være blank"
 
Writeline "Vis menu, 1 for ja, 2 for nej"

## Øvelse 3.3: Lav pseudokode for SelectMenuItem()-beskeden i Menu-objektet
   
 
## Øvelse 3.1: Lav pseudokode for Main()-beskeden i Program-klassen
   

Main kalder på klassen Menu  
Et loop starter der tilføjer et MenuItem  
Menu laver en string med menuItemTitle  
ItemCount stiger med 1  
ShowMenu metoden kører og viser menuItemTitle  
Brugeren bliver bedt om at vælge et tal fra menuen
 
Et menu selection loop starter og kører så længe itemId er større end nul  
Et If else eller switch statement starter  
Udfra brugervalg viser konsollen hvad der svarer til hvert itemId  
If else eller switch slutter  
Loopet vender tilbage til start og bruger kan vælge et nyt punkt
   

## Øvelse 3.2: Lav pseudokode for Show()-beskeden i Menu-objektet
   

Læs inputtet  
Konverter input til bool  
Clear console  
If else statement starter  
Hvis show er true kører ForEach loop  
ForEach loop med MenuItem starter  
Writeline menuItem.Title udskrives  
ForEach loop lukker
 
Writeline med "Help" til konsollen"  
If else */
 
Show metoden kører  
Ryd konsol vinduet  
Beder om bruger input til at vælge menuItem  
Input gemmes i en int som bruges til at finde det korrekte menu valg  
ForEach loop iterere over menuItem  
Den menuItemTitle der svarer til brugers int udskrives  
ForEach loop slutter
 
Writeline med HelpText udskrives til konsollen
   

## Øvelse 3.3: Lav pseudokode for SelectMenuItem()-beskeden i Menu-objektet
 
I klassen :Menu starter SelectMenuItem metoden  
ItemId starter som invalid  
Brugerens valg af menu int læses i konsol vinduet  
If id er et tal  
Konverter til itemId int  
If itemId er større eller mindre end 0 og itemId er mindre end itemCount  
ItemId er valid  
Else  
ItemId er invalid  
Writeline error message i konsollen  
Returner itemId