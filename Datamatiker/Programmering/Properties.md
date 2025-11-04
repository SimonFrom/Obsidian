[Get & Set](https://csharp.net-tutorials.com/classes/properties/)
 
Den normale måde at lave et get/set field på---------------------------------\>  
Hvis man kun har get eller set er man nødt til at bruge denne måde at lave get/set på.
                
Disse properties er gode til at styre hvad ens variabler bliver skiftet ud med inde i klassen -----------------\>
                     

Der er forskellige måder at angive get/set på.  
Se eksempler --------------------------------------\>
   

Man kan også lave så kaldte side effekter i sine properties:

![Exported image](Exported%20image%2020251104230552-0.png)  

Her setter man firstName, men derefter setter man også fullName med det netop tilføjede firstName.
    
![Exported image](Exported%20image%2020251104230553-1.png) ![Exported image](Exported%20image%2020251104230554-2.png) ![Exported image](Exported%20image%2020251104230558-3.png) ![Exported image](Exported%20image%2020251104230559-4.png) ![Exported image](Exported%20image%2020251104230600-5.png) ![Exported image](Exported%20image%2020251104230601-6.png) ![Exported image](Exported%20image%2020251104230604-7.png)

Læg mærke til =\> mellem get og name

Auto-implemented properties kan bruges når der ikke er behov at yderligere specifikation.  
Der er ikke deklareret et backing field.