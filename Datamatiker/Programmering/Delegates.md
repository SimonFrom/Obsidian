[Delegates](https://www.youtube.com/watch?v=6NRqwL3N5Go&ab_channel=Tarodev)
   

En delegate er en variabel der indeholder en metode i stedet for data.  
En delegate kan også beskrives som signaturen for en metode.  
Definér delegate type i namespace, før klassen.  
Tilføj metoder, der matcher med metodesignaturen i klassen.  
Delegates associeres med en metode, der har en kompatibel metodesignatur.
   

- **Anonymous delegates:** En anonym delegate er en delegate, der defineres inline (uden et navngivet metodekald) og bruges til at definere en metode direkte på stedet.
- **Composable delegates:** Composable delegates henviser til muligheden for at "samle" flere dele af funktionalitet i én delegate. Du kan tilføje flere metoder til en delegate, og når denne delegate kaldes, udføres alle metoder i rækkefølge.
- **Pass by reference** betyder, at du sender en reference til en metode i stedet for at sende en kopi af objektet. Når et objekt sendes by reference, kan den kaldte metode ændre på den oprindelige værdi eller objekt, der blev sendt ind i metoden.
      
![Med andre ord ser koden sdan her ud delegate int x...](Exported%20image%2020251104230639-0.png)  

Anonym Delegate:

![Med andre ord ser koden sdan her ud public delegat...](Exported%20image%2020251104230641-1.png)  

En anonym delegate kan koges ned med lambda expressions:

![Hvad er det nye Fra anonym metode til lambda expre...](Exported%20image%2020251104230645-2.png)  
![Med andre ord ser koden sdan her ud namespace Insi...](Exported%20image%2020251104230646-3.png)  
![void arg y y indbygget delegate Sut VOrS expressio...](Exported%20image%2020251104230647-4.png)