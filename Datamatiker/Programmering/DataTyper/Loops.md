[ForEach Loop](https://codebuns.com/csharp-basics/foreach-loop/)  
[ForEach, C# School](https://www.w3schools.com/cs/cs_foreach_loop.php)  
[Nested Loops](https://www.youtube.com/watch?v=WFzLcZk137s&list=PLZPZq0r_RZOPNy28FDBys3GVP2LiaIyP_&index=18&ab_channel=BroCode)  
[Nested Loop GPT Chat](https://chatgpt.com/c/677f9910-7cc4-8006-8259-657562b670ff)
   

## For loop:
 ![for int i e i le WriteLine Console 1 Hello](Exported%20image%2020251104230802-0.png)

## Do While:
 ![class Forever public static void Main do Console W...](Exported%20image%2020251104230806-1.png)

## While:
 ![class WhileLoopInsteadOfFor public static void Mai...](Exported%20image%2020251104230808-2.png)     

## ForEach loop:
 
ForEach loopet bruges ofte til at gå igennem arrays enten for at tjekke værdier eller for at udskrive forskellige værdier.  
Det kan kun læse, ikke skrive.  
Man kan også bruge break og continue statements.

- Med break kan man stoppe loopet ved at lave et if(value == "Ford) statement inden i kroppen.
- Med continue kan man springer værdier over med samme syntax som ovenover. Her vil den springe hele kode kroppen over og vende tilbage til start hvis betingelsen er mødt.
 
Eksempel på at udskrive alle værdier fra et array:

![Exported image](Exported%20image%2020251104230809-3.png)  

## Nested Loop:
 
Bliver ofte brugt i sorterings algoritmer.
 ![Exported image](Exported%20image%2020251104230810-4.png)  

**Forklaring:**

1. **Ydre løkke** (for (int i = 0; i \< array.Length - 1; i++)):
    1. Denne løkke kører én gang for hver passering af hele arrayet. Ved hver passering placeres det største element fra de usorterede dele af arrayet i den korrekte position.
      
    
2. **Indre løkke** (for (int j = 0; j \< array.Length - i - 1; j++)):
    1. Denne løkke sammenligner elementer parvis og bytter dem, hvis de ikke er i korrekt rækkefølge.
      
    
3. **Sammenligning** (if (array[j] \> array[j + 1])):
    1. Hvis det første element er større end det næste, byttes de.
      
    
4. **Bytning:**
    1. Der bruges en midlertidig variabel **temp** til at holde værdien af det første element, mens det byttes.