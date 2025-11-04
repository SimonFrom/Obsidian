typeof-operatoren i C# bruges til at få en System.Type-instans for en given type. ==Dette er nyttigt, når du har brug for at arbejde med metadata om typer ved runtime, såsom at få navnet på typen, dens namespace, eller dens metoder og egenskaber==.
 
**Eksempel på brug af** ==typeof==**-operatoren:**
 ![Exported image](Exported%20image%2020251104230851-0.png)  

==I dette eksempel returnerer typeof(string) en Type-instans, der repræsenterer System.String-typen. FullName-egenskaben bruges til at udskrive det fulde navn på typen. Her vil den udskrive Type: System.String.==  
Dette skyldes, at typeof(string) returnerer en Type-instans, der repræsenterer System.String-typen, og FullName-egenskaben bruges til at udskrive det fulde navn på typen.
 
**Anvendelser af** ==typeof==**:**

- **Få metadata om en type**==: Som vist i eksemplet ovenfor.==
- **Refleksion**==: Bruges ofte i refleksion til at inspicere typer ved runtime.==
- **Generiske metoder**==: Kan bruges til at få typen af generiske parametre.==