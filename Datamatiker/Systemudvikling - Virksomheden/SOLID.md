[What is SOLID?](https://contabo.com/blog/what-are-solid-principles/)
   

SOLID er et sæt principper der kan bruges til at skrive kode der er:

- Skalerbar
- Fleksibel
- Nem at vedligeholde
 
SOLID står for:  
S - Single Responsibility Principle  
O - Open/Closed Principle  
L - Liskov Substitution Principle  
I - Interface Segregation Principle  
D - Dependency Inversion Principle
 
### Single Responsibility Principle:

Dette beskriver at en klasse kun skal have en grund til at ændre sig.  
Dette skal forstås som at en klasse kun bør have ansvar for en ting eller udføre en opgave.
 
### Open/Closed Principle:

Her beskrives der at software entiteter (klasser, metoder eller moduler) skal være åbne for udvidelser, men lukkede for modifikationer. Det kan opnås gennem nedarvning, abstraktion eller polymorfisme.
 
### Liskov Substitution Principle:

Liskov Substitution Principle siger, at objekter af en afledt klasse skal kunne bruges i stedet for objekter af deres baseklasse uden at ændre korrektheden af programmet.  
Her kan man bruge et interface og abstrakte klasser til at styre implementeringen af objekter igennem programmet.
 
```
//En abstrakt klasse som kan   
//bruges som base til andre objekter  
public abstract class Bird   
{  
    string Species { get; }  
    int Weight { get; }  
}  
//Et interface til at bestemme hvad flyvende fugle skal indeholde  
public interface IFlyingBird  
{  
    void Fly();  
}  
//Den flyvende fugl implenterer både Bird og IFlyingBird  
public class Sparrow : Bird, IFlyingBird  
{  
    flightTime = 10  
    public void Fly() =\> Console.WriteLine("Sparrow is flying");  
}  
//Den ikke flyvende bruger kun Bird klassen,  
//og lover ikke nogen flyve funktionalitet  
public class Ostrich : Bird  
{  
    // Ingen flyveevne  
}
```
 
### Interface Segregation Principle:

ISP omhandler at interfaces kun skal indeholde operationer der er relevante og helst holdes korte. Dette fremhæver 'loose coupling' og 'high cohesion'.
 
### Dependency Inversion Principle:

Forestil dig en **DataProcessor**-klasse, der skal interagere med en database. I stedet for at afhænge direkte af en specifik database-implementering (som f.eks. **MySQL**), bør den afhænge af et database-interface (f.eks. **DatabaseInterface**). På den måde kan forskellige databaseimplementeringer bruges, og **DataProcessor** kan arbejde med dem alle gennem interfacet – uden at det er nødvendigt at ændre i dens kode.