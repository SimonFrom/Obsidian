[BroCode/Abstrakte klasser](https://www.youtube.com/watch?v=06BrbJm7Sho)  
[C# Abstraction](https://www.w3schools.com/cs/cs_abstract.php)  
[Override modifier](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/override)  
[Static classes and staic class members](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/static-classes-and-static-class-members)
    
En klasse i C# er en brugerdefineret datatype, der fungerer som en skabelon til at skabe objekter. Den beskriver egenskaber (felter og egenskaber) og adfærd (metoder), som objekter af klassen kan have. Klassen er grundlaget for objektorienteret programmering (OOP) i C#.
 
En klasse defineres med nøgleordet class og klasse navn.  
Den kan indeholde:

- Felter: Private variabler, der repræsenterer objektets data.
- Egenskaber: Offentlige adgangspunkter for felterne.(Access modifier)
- Metoder: Funktioner, der definerer objektets adfærd. (Klassekrop)
- Konstruktører: Specielle metoder, der bruges til at initialisere objekter.
 
# Abstrakt klasse:

En abstrakt klasse i C# er en klasse, der ikke kan instantieres direkte, men som kan være en base for andre klasser. Abstrakte klasser bruges til at definere fælles funktionalitet og metoder, som kan implementeres af afledte klasser. En abstrakt metode er en metode, der ikke har nogen implementering i den abstrakte klasse, men som skal implementeres i de afledte klasser.  
Hovedtræk ved abstrakt klasse

- Kan ikke instantieres direkte: Du kan ikke oprette et objekt af en abstrakt klasse. Du kan kun oprette objekter af konkrete (afledte) klasser, der implementerer de abstrakte metoder.
- Kan indeholde både abstrakte og konkrete metoder: Abstrakte klasser kan indeholde både metoder, der ikke har implementering (abstrakte metoder), og metoder, der har en implementering.
- Kan indeholde felter, egenskaber, konstruktører og metoder: En abstrakt klasse kan have felter og egenskaber, ligesom en almindelig klasse, og de kan bruges af afledte klasser.
- Forskellige adgangsniveauer: Medlemmer kan være public, private, protected osv.
- En abstrakt klasse: Klassenavn er i kursiv i UML
 ![Exported image](Exported%20image%2020251104230922-0.png)   ![Exported image](Exported%20image%2020251104230923-1.png)     

# Statisk klasse:

En statisk klasse kan ikke instantieres, altså kan man ikke bruge "new" keywordet til at lave en ny variabel af klassens type.  
I stedet vil man kalde klassen og derefter metoden:

![Exported image](Exported%20image%2020251104230925-2.png)  

Her hedder den statiske klasse UtilityClass og metoden A.  
Statisk klasser er gode når der ikke skal opbevares nogle værdier i klassen, men den kun skal opererer med input og output. Math klassen er et godt eksempel. Her bliver der kun lavet beregninger.
 
# Static members:

Statiske værdier er gode når der skal deles en værdi mellem forskellige klasser eller metoder eller til at holde en konstant optælling. Der vil altid kun eksisterer en udgave af værdien.  
En statisk værdi kaldes ligesom klassen ovenover:  
UtilityClass.StaticIntCount;