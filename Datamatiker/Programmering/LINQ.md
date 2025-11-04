[LINQ Basics](https://www.tutorialsteacher.com/linq/what-is-linq)  
[LINQ Query operators](https://www.tutorialsteacher.com/linq/linq-standard-query-operators)  
[Microsoft LINQ](https://learn.microsoft.com/en-us/dotnet/api/system.linq?view=net-9.0)
   

LINQ (Language Integrated Query) er en del af .NET og giver en fleksibel måde at forespørge og manipulere data på tværs af forskellige datakilder som lister, arrays, databaser og XML. LINQ gør brug af metoder og syntaks, der minder om SQL, hvilket gør kode mere læsbar og forståelig.
 
Der er to primære måder at skrive LINQ på:
 
- **Query syntax** (SQL-lignende)
- **Method syntax** (Bruger metoder som .Where(), .Select(), osv.)
 
Eksempler:
 
### 1. Filtrering med LINQ (Where)

Her filtrerer vi en liste af tal, så vi kun får de lige tal:
 
Method syntax:
 ![Exported image](Exported%20image%2020251104231030-0.png)  

```
Query syntax:
```
 ![Exported image](Exported%20image%2020251104231031-1.png)  

### 2. Projektion med LINQ (Select)

Vi henter kun navnene fra en liste af personer:
 ![Exported image](Exported%20image%2020251104231032-2.png)  

### 3. Sortering med LINQ (OrderBy)

Sorterer en liste af tal i stigende rækkefølge:
 ![Exported image](Exported%20image%2020251104231038-3.png)