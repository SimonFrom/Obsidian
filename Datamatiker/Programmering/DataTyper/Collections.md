[Geeks For Geeks](https://www.geeksforgeeks.org/collections-in-c-sharp/)
 
I C# er collections datastrukturer, der bruges til at gemme, håndtere og manipulere grupper af objekter. Collections findes i **System.Collections** og **System.Collections.Generic** namespaces og tilbyder fleksible måder at arbejde med lister, sæt, køer osv.  
Typer af Collections:

1. List\<T\> (Generisk liste):
    - Dynamisk liste, hvor elementer kan tilføjes eller fjernes.
    - Eksempel: List\<int\> numbers = new List\<int\>();
2. Dictionary\<TKey, TValue\>:
    - Lagrer nøgle-værdi par, hvor hver nøgle er unik.
    - Eksempel: Dictionary\<int, string\> students = new Dictionary\<int, string\>();
3. Queue\<T\>:
    - FIFO (First In, First Out) datastruktur.
    - Eksempel: Queue\<string\> tasks = new Queue\<string\>();
4. Stack\<T\>:
    - LIFO (Last In, First Out) datastruktur.
    - Eksempel: Stack\<int\> stack = new Stack\<int\>();
5. HashSet\<T\>:
    - Lagrer unikke elementer uden en bestemt rækkefølge.
    - Eksempel: HashSet\<string\> set = new HashSet\<string\>();

Generiske collections (som List\<T\>, Dictionary\<TKey, TValue\>) er type-sikre og giver bedre ydeevne og fleksibilitet end ikke-generiske collections.
 
![Exported image](Exported%20image%2020251104230728-0.png)