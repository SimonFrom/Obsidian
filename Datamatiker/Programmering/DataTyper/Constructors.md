## [Constructor Overloading:](https://www.geeksforgeeks.org/c-sharp-constructor-overloading/)

Constructor overloading i C# betyder, at en klasse kan have flere konstruktører med forskellige parametre. Dette giver mulighed for at oprette objekter på forskellige måder afhængigt af de argumenter, der gives ved instansiering.
 
**Eksempel:**

![Exported image](Exported%20image%2020251104230814-0.png)  

Når en klasse har flere konstruktører, vælger C# den, der matcher antallet og typen af de argumenter, der gives ved oprettelsen af objektet. Dette giver fleksibilitet i, hvordan objekter kan oprettes.
 
## [Constructor Chaining:](https://www.c-sharpcorner.com/UploadFile/825933/constructor-chaining-in-C-Sharp/)
 
Constructor chaining i C# er en teknik, hvor én konstruktør kalder en anden konstruktør i samme klasse (eller i en baseklasse) for at genbruge kode og undgå redundans. Dette gøres ved at bruge nøgleordet "this" (for samme klasse) eller "base" (for baseklasse). Det hjælper med at centralisere initialisering i én konstruktør, hvilket forbedrer vedligeholdelsen.
 
**Eksempel:**

![Exported image](Exported%20image%2020251104230816-1.png)  

I dette eksempel kalder den parameterløse konstruktør den anden konstruktør ved hjælp af this og sender standardværdier. Constructor chaining gør det nemmere at undgå duplikeret kode og giver fleksibilitet i, hvordan objekter kan initialiseres.