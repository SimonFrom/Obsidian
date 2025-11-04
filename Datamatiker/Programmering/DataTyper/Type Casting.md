[TypeCasting](https://www.youtube.com/watch?v=uajWePMMs84&ab_channel=BroCode)
    
Type casting i C# refererer til processen med at konvertere et objekt fra en datatype til en anden. Der er to typer type casting: implicit og eksplicit.  
**Implicit casting**  
==Implicit casting sker automatisk, når du konverterer en mindre datatype til en større datatype. Dette sker uden tab af data.==  
==Eksempel:==
 ![Exported image](Exported%20image%2020251104230837-0.png)  

**Eksplicit casting**  
==Eksplicit casting kræver, at du manuelt angiver den ønskede datatype i parentes foran værdien. Dette er nødvendigt, når der kan være tab af data, eller når konverteringen måske ikke lykkes.==  
==Eksempel:==
 ![Exported image](Exported%20image%2020251104230839-1.png)  

**Brug af** ==is==**-operatoren til type testing**  
==For at sikre, at en type casting er sikker, kan du bruge is-operatoren til at teste kompatibiliteten, før du udfører castingen.==  
==Eksempel:==

![Exported image](Exported%20image%2020251104230844-2.png)  

==Med GetType i sit WriteLine statement kan udskrive typen på argumentet.==
 
==Eksempel:==

![Exported image](Exported%20image%2020251104230846-3.png)

==Output = System.Double==