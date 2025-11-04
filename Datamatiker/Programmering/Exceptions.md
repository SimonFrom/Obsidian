BroCode  
Microsoft  
Exception class and properties  
Exception Handling Guide Try Catch  
Exception handling statements  
Best practices for exceptions  
How to create user-defined exceptions  
Handling exceptions (Bob Tabor Video)
   

Exceptions kan bruges når man vil køre noget kode som kan anses for at være "farlig", det skal forstå som noget der kan tvinge programmet til at stoppe eller fejle på anden måde.
 
En exception (undtagelse) i C# er en hændelse, der opstår, når noget går galt i programmet, og koden ikke kan fortsætte med at køre som forventet. Det kan f.eks. være forsøg på at åbne en fil, der ikke findes, forsøge at dividere med nul, eller få adgang til et array med et ugyldigt indeks.
 
Exceptions:

- NotImplementedException ”Programmøren har endnu ikke skrevet koden”
- NotSupportedException ”Jeg kommer aldrig til at kunne gøre dette igen”
- InvalidOperationException ”Jeg kan ikke gøre dette i min nuværende state, men måske i en anden state”
- ArgumentOutOfRangeException ”Argumentet var for stort (eller for småt) for at jeg kan bruge det”
- FormatException ”Argumentet var ikke i det ønskede format”
- DivideByZeroException ”Man kan ikke dividere med 0!”
- ArgumentNullException ”Argumentet var null og jeg kan ikke arbejde med en null værdi”
- ArgumentException ”Noget er galt med en af dine argumenter”
- Exception ”Noget gik galt, og jeg har ingen info om hvad der gik galt”
   

### **Try:**

- Med Try lukker man den mængde kode inde i Tuborg klammer som man vurderer at være tvivlsom.

### **Catch:**

- Med Catch fanger man den exception man forventer dukker op også kaldet "exception filter", divider med 0 i en lommeregner f.eks.
- Man vil altid starte med at søge efter de mest specifikke exceptions og videre til de mere generelle og eventuelt et Catch All statement som vil fange alle exceptions der slipper igennem med en default besked ala "Der opstod et problem!".

### **Finally:**

- Med Finally kan indsætte kode som kører uanset hvad der sker når man kommer igennem.

Finally statements er valgfri at have med. Kan eventuelt bruges til at lukke en streamreader.