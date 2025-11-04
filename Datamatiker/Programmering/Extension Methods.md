[How to implement and call a custom extension method (C# Programming Guide)](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-and-call-a-custom-extension-method)  
[Extension Methods LinkedIn](https://www.linkedin.com/learning/c-sharp-essential-training-1-syntax-and-object-oriented-programming/extension-methods?u=57075649)  
[Extension Methods YouTube](https://www.youtube.com/watch?v=iI9sfsMIZE8&ab_channel=tutorialsEU-C%23)
 
Extension methods er en måde at udvide klasser der normalt er lukkede eller ikke tilgængelige.
 ![Exported image](Exported%20image%2020251104230941-0.png)  

Her et eksempel hvor man tilføjer til string klassen, som normalt hverken er tilgængelig eller mulig at ændre.
 
Defineres som andre metoder, men skal have "this" keywordet ind med input parametre.  
Den skal også være static, ligesom klassen den befinder sig i skal. Static gør blandt andet at man ikke behøver  
en instans af klassen den befinder sig i.
 ![Exported image](Exported%20image%2020251104230942-1.png)  

Eksempel på brug af metoden fra oven. ￼Using keywordet bruges hvis det er nødvendigt og ellers kan man bruge metoden som normalt.