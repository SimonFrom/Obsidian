- Lambda Functions: Lambdas are shorthand for writing anonymous functions in C#. They are based on delegates and can be used wherever a delegate function is used.
- Arrow Operator: Lambdas use the arrow operator (=\>) to define the function. The left side of the arrow contains the input parameters, and the right side contains the expression or statements.

Types of Lambdas:

- Expression Lambdas: Consist of a single expression on the right side of the arrow.
- Statement Lambdas: Contain multiple statements enclosed in curly braces.

![Exported image](Exported%20image%2020251104230606-0.png)

[Jon Skeet - Lambda](https://drive.google.com/drive/folders/1MT5tJtIigTEZ4IKoFXo4aTXau1sxlr75?ths=true)  
[Action, Func and Predicate](https://www.infoworld.com/article/2250162/how-to-work-with-action-func-and-predicate-delegates-in-c-sharp.html)  
[Microsoft Lambda](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions)

Expression

- **Lambda** **expressions** i C# er en kortere syntaks for at definere anonyme metoder (funktioner) inline. De er ofte brugt sammen med LINQ, delegater, og events til at skrive mere kompakt og læsbar kode.
- **Parameters**: En eller flere inputparametre, der sendes til lambdaen (kan være tomme).
- =\>: En "lambda operator", der adskiller parametrene fra koden, der udføres.
- **Expression**: Koden (eller udtrykket), der køres. Hvis det er en enkelt linje, returneres resultatet implicit.

Statement

- Lambda expressions kan enten være expression-bodied (en enkelt linje) eller statement-bodied (flere linjer).
- **Expression**-**bodied** **Lambda** (enkelt linje):
    - Anvendes, når der kun er en enkelt linje kode i lambdaen.
    - Det returnerede resultat bestemmes implicit
- **Statement**-**bodied** **Lambda** (flere linjer):
    - Anvendes, når du har flere operationer i lambdaen, og du har brug for at udføre dem inden returnering af resultatet.
    - Du skal bruge {} til at definere flere linjer, og du skal bruge et explicit return statement for at returnere et resultat, hvis lambdaen har en returværdi.

**Indbyggede delegate typer:**

- Action
    - Returnerer ikke nogen værdi (void)
    - Kan tage nul eller flere inputparametre
- Func
    - Skal returnere en værdi
    - Kan tage nul eller flere inputparametre
    - Først angives eventuelle inputparametre, derefter returtypen
- Predicate
    - Kan kun returnere bool
    - Kan kun have én inputparameter