[Normaliserings Former:](https://www.linkedin.com/learning/programming-foundations-databases-2/normalization-2?u=57075649)

Normaliserings former i SQL-databaser bruges til at organisere data for at reducere redundans og forbedre dataintegritet. Her er en kort beskrivelse af de vigtigste normalformer:

**1. Normalform (1NF)**  
Alle attributter skal være enkelt værdier (ingen lister eller arrays i en enkelt kolonne).  
Hver række skal have en unik identifikator (primær nøgle).  
Der må ikke være duplikerede rækker og kolonner.

**2. Normalform (2NF)**  
Skal opfylde 1NF.  
Alle ikke-nøgle attributter skal være fuldstændigt funktionelt afhængige af hele primærnøglen (ingen delvise afhængigheder i sammensatte nøgler).

**3. Normalform (3NF)**  
Skal opfylde 2NF.  
Ingen ikke-nøgle attribut må afhænge af en anden ikke-nøgle attribut:  
En værdi må ikke være afhængig af en anden, nedenunder vil man kunne ændre LunchPrice til noget andet end halvdelen af Price.  

**Boyce-Codd Normalform (BCNF)**  
En stærkere version af 3NF.  
Hver determinant skal være en kandidatnøgle (hvis en ikke-nøgle attribut afhænger af en delmængde af en kandidatnøgle, skal tabellen opdeles).

**4. Normalform (4NF)**  
Skal opfylde BCNF.  
Ingen multiværdiafhængigheder (ingen attribut må afhænge af mere end én værdi uden at være relateret til den primære nøgle).

**5. Normalform (5NF) / Projektion-Sammensætning Normalform (PJNF)**  
Skal opfylde 4NF.  
Fjerner alle resterende anomalier, der kan løses ved at opdele tabeller uden at miste data.  
De fleste praktiske databaser holder sig til 3NF eller BCNF, da det giver en god balance mellem normalisering og ydeevne.
 

 



 

 


**Denormalization:**  
Denormalisering er processen med bevidst at bryde normaliseringsreglerne for at forbedre ydeevnen i en database. Selvom normalisering reducerer redundans og forbedrer dataintegritet, kan det føre til mange join-operationer, hvilket kan gøre forespørgsler langsomme.

**Hvorfor denormalisere?**  
Forbedre læsehastighed: Mindsker behovet for komplekse joins, hvilket kan give hurtigere forespørgsler.  
Reducerer beregningskrav: Nogle beregninger kan forudlagres i en denormaliseret tabel for at spare tid.  
Optimering til rapportering: Datawarehouse-systemer bruger ofte denormalisering for at forbedre performance i analytiske forespørgsler.


