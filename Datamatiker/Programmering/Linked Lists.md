[Linked Lists](https://www.tutorialspoint.com/data_structures_algorithms/linked_list_algorithms.htm)  
[Linked lists video](https://www.linkedin.com/learning/learning-c-sharp-algorithms/linked-list-explained?u=57075649)  
[BroCode Linked Lists](https://www.youtube.com/watch?v=N6dOwBde7-M)
 
​En linked list er en lineær datastruktur, hvor elementer, kaldet noder, er forbundet via pointere. Hver node indeholder to dele: data, som repræsenterer værdien, og en pointer, der peger på den næste node i sekvensen. I modsætning til arrays, hvor elementerne lagres i sammenhængende hukommelsespladser, er noderne i en linked list spredt i hukommelsen og forbundet gennem disse pointere. ​
 
## Typer af Linked Lists:
 
**Singly Linked List**: Hver node har en enkelt pointer, der peger på den næste node. Dette tillader bevægelse i én retning, fra starten (head) til slutningen (tail) af listen. ​
 
**Doubly Linked List**: Hver node indeholder to pointere: én, der peger på den næste node, og én, der peger på den foregående node. Dette muliggør bevægelse i begge retninger. ​
 
**Circular Linked List**: I denne type peger den sidste node tilbage på den første node, hvilket danner en cirkulær struktur. Dette kan implementeres både i singly og doubly linked lists. ​
   

## Fordele ved Linked Lists:
 
**Dynamisk størrelse**: Linked lists kan vokse eller skrumpe under programmets kørsel, da hukommelse allokeres eller frigives efter behov. ​
 
**Effektiv indsættelse og sletning**: Tilføjelse eller fjernelse af noder kræver kun opdatering af pointere, hvilket gør disse operationer mere effektive sammenlignet med arrays, hvor elementer muligvis skal flyttes. ​
 
## Ulemper ved Linked Lists:
 
**Sekventiel adgang**: For at få adgang til en bestemt node skal man gå igennem listen fra starten, hvilket kan være tidskrævende sammenlignet med arrays, der tillader direkte adgang via indeks. ​
 
**Ekstra hukommelsesforbrug**: Hver node kræver ekstra hukommelse til at lagre pointere, hvilket øger det samlede hukommelsesforbrug. ​
 
## Anvendelser af Linked Lists:
 
**Implementering af andre datastrukturer**: Linked lists bruges ofte til at implementere datastrukturer som stacks, queues, grafer og hash maps. ​
 
**Effektiv håndtering af dynamiske data**: De er velegnede til scenarier, hvor antallet af elementer ændrer sig ofte, da de tillader fleksibel hukommelsesallokering og -frigivelse. ​
 
Samlet set tilbyder linked lists en fleksibel og effektiv måde at håndtere dynamiske datasæt på, især når det kommer til indsættelse og sletning af elementer. Dog skal man være opmærksom på deres begrænsninger, især med hensyn til adgangstid og hukommelsesforbrug.
 ![Exported image](Exported%20image%2020251104231045-0.png)