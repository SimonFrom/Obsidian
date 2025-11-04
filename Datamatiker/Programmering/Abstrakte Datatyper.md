[Abstract datatypes](https://www.linkedin.com/learning/python-data-structures-linked-lists/abstract-data-types-17347692?u=57075649)
 
Abstrakte datatyper (ADT'er) i forhold til C# refererer til datastrukturer, der definerer, hvordan data kan tilgås og manipuleres, men skjuler implementeringsdetaljerne. ADT'er specificerer kun operationer og adfærd uden at angive, hvordan de er implementeret. Dette følger abstraktionens princip i objektorienteret programmering.
 
✅ Nøgleelementer i abstrakte datatyper

- **Data**: Hvad der gemmes (f.eks. en liste af tal).
- **Operationer**: Hvilke handlinger kan udføres (f.eks. tilføjelse, fjernelse, søgning).
- **Skjult** **Implementering**: Hvordan data gemmes og operationer udføres er skjult for brugeren.
 ![Exported image](Exported%20image%2020251104231041-0.png)  

## Oprettelse af en brugerdefineret ADT

Du kan lave dine egne abstrakte datatyper ved at bruge abstrakte klasser eller interfaces i C#. For eksempel en simpel liste-ADT:
 ![Exported image](Exported%20image%2020251104231042-1.png)   
## Fordele ved at bruge ADT'er i C#:

- **Abstraktion**: Skjuler komplekse implementeringsdetaljer.
- **Genanvendelighed**: Standardiserede datastrukturer kan bruges på tværs af projekter.
- **Vedligeholdelse**: Nem at udskifte implementering uden at ændre resten af koden.