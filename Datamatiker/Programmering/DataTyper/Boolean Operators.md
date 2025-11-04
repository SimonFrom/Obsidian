Boolske operatorer i C# bruges til at arbejde med logiske værdier (sand/falsk, også kaldet true/false).
 
De vigtigste boolske operatorer i C# er:
 
- && (AND): Returnerer true, hvis begge operander er sande. Fx: true && false returnerer false.
- || (OR): Returnerer true, hvis mindst én operand er sand. Fx: true || false returnerer true.
- ! (NOT): Inverterer den logiske værdi. Fx: !true returnerer false.
- ^ (XOR): Returnerer true, hvis kun én af operanderne er sand. Fx: true ^ false returnerer true.
- AS bruges til at konvertere en type til en anden kompatibel type.

```
Object obj = Hello;  
String str1 = obj as string;  
Console.Writeline(str1); //Output: Hello
```
 
Boolske operatorer bruges ofte i kontrolstrukturer som if-sætninger for at evaluere logiske udtryk og træffe beslutninger baseret på resultatet.