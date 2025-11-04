[Benefit of structured databases](https://www.linkedin.com/learning/programming-foundations-databases-2/understanding-databases-benefits-of-structured-data?contextUrn=urn%3Ali%3AlyndaLearningPath%3A5ee163eb498e92010af0dcbe&u=57075649)  
[Benefits of spreadsheets](https://www.linkedin.com/learning/programming-foundations-databases-2/understanding-databases-benefits-of-spreadsheets?contextUrn=urn%3Ali%3AlyndaLearningPath%3A5ee163eb498e92010af0dcbe&u=57075649)  
[Relational databases](https://www.linkedin.com/learning/programming-foundations-databases-2/relational-databases-2?contextUrn=urn%3Ali%3AlyndaLearningPath%3A5ee163eb498e92010af0dcbe&u=57075649)  
[Key and unique values](https://www.linkedin.com/learning/programming-foundations-databases-2/keys-and-unique-values-2?contextUrn=urn%3Ali%3AlyndaLearningPath%3A5ee163eb498e92010af0dcbe&u=57075649)  
[Relationships](https://www.linkedin.com/learning/programming-foundations-databases-2/relationships-2?contextUrn=urn%3Ali%3AlyndaLearningPath%3A5ee163eb498e92010af0dcbe&u=57075649)  
[W3 Schools SQL Tutorial](https://www.w3schools.com/sql/)  
[SqlBolt](https://sqlbolt.com/lesson/introduction)  
[Relational Model Wiki](https://en.wikipedia.org/wiki/Relational_model#Database)  
[SQL Datamapping/Conversion](https://learn.microsoft.com/en-us/sql/connect/ado-net/sql-server-data-type-mappings?view=sql-server-ver16)  
[SQL AutoIncrement](https://www.w3schools.com/sql/sql_autoincrement.asp)  
[SQL Constraints](https://www.w3schools.com/sql/sql_constraints.asp)  
[SQL Primary Key](https://www.w3schools.com/sql/sql_primarykey.asp)  
[SQL Foreign Key](https://www.w3schools.com/sql/sql_foreignkey.asp)  
[SQL Not Nul](https://www.w3schools.com/sql/sql_notnull.asp)  
[Database normalization](https://www.youtube.com/watch?v=GFQaEYEc8_8)  
[StoredProcedures](https://www.youtube.com/watch?v=fjNsRV4zLdc) Se part 2 også  
[SQL Data Definition Language](https://www.geeksforgeeks.org/sql-ddl-dql-dml-dcl-tcl-commands/)
 ![Exported image](Exported%20image%2020251104231021-16.png) ![Exported image](Exported%20image%2020251104231020-15.png) ![Exported image](Exported%20image%2020251104231019-14.png)        

```
DROP PROC spOwnerNameID
```

### Sletning af SP:
 
```
USE PET_PARADISE  
GO  
ALTER PROC spOwnerNameID ----- CREATE skiftes ud med ALTER  
AS  
BEGIN  
SELECT OwnerId,  
LastName,  
Phone  
FROM OWNER  
ORDER BY FirstName ASC  
END
```
 
Den ønskede procedure findes i solution explorer, højre klik og vælg Modify.

### Ændring af SP:
 
```
EXEC spOwnerNameID 1, 5, James
```

### Flere parametre:

```
EXEC spOwnerNameID 1 ------ 1 parameteren som man ønsker at bruge i sin clause
```

### Brug af SP med parameter:
 
### ￼EXEC spOwnerNameID ------ EXEC kan også skrives som EXECUTE

### Brug af SP:
       
```
ALTER PROC spShowServicePricePetID  
(  
@MinPrice AS INT = NULL, ----------- Her angives en standard værdi for MinPrice, denne kan ændres.  
@MaxPrice AS INT = NULL  
)  
AS  
BEGIN  
SELECT PetID,  
Service,  
Charge  
FROM TREATMENT  
WHERE  
(@MinPrice IS NULL OR Charge \>= @MinPrice) AND ---- VIGTIGT! Der skal tjekkes for NULL for at det virker  
(@MaxPrice IS NULL OR Charge \<= @MaxPrice)  
ORDER BY Charge DESC  
END
```

### SP med valgfrie parametre:
 
```
CREATE PROC spOwnerNameID  
(	  
@StartID AS INT,  
@EndID AS INT,  
@Name AS VARCHAR(MAX)  
)  
AS  
BEGIN  
SELECT OwnerId,  
LastName,  
Phone  
FROM OWNER  
WHERE   
OwnerId \> @StartID AND  
OwnerId \< @EndID AND  
LastName LIKE '%' + @Name + '%' ---- % er wildcards  
ORDER BY OwnerId ASC  
END
```

### Oprettelse af SP med flere parametre:
 
```
USE PET_PARADISE   
GO  
CREATE PROC spOwnerNameID(@ID AS INT) ------- Her angives hvad der skal kunne puttes ind som parameter  
AS  
BEGIN  
SELECT OwnerId,  
LastName,  
Phone  
FROM OWNER  
WHERE OwnerId = @ID ------ Her bruger man parameteren f.eks i WHERE clause  
ORDER BY FirstName ASC  
END
```

### Oprettelse af SP med parameter:
 
```
USE PET_PARADISE ---- God ide at angive hvilken db der skal bruges, men ikke strengt nødvendigt  
GO  
CREATE PROC spOwnerNameID  
AS  
BEGIN  
SELECT OwnerId,  
LastName,  
Phone  
FROM OWNER  
ORDER BY FirstName ASC  
END
```

### Oprettelse af SP:
 ![Exported image](Exported%20image%2020251104231017-13.png)  

# Stored Procedures
      

**5. Normalform (5NF) / Projektion-Sammensætning Normalform (PJNF)**  
Skal opfylde 4NF.  
Fjerner alle resterende anomalier, der kan løses ved at opdele tabeller uden at miste data.  
De fleste praktiske databaser holder sig til 3NF eller BCNF, da det giver en god balance mellem normalisering og ydeevne.
 
**4. Normalform (4NF)**  
Skal opfylde BCNF.  
Ingen multiværdiafhængigheder (ingen attribut må afhænge af mere end én værdi uden at være relateret til den primære nøgle).
 
**Boyce-Codd Normalform (BCNF)**  
En stærkere version af 3NF.  
Hver determinant skal være en kandidatnøgle (hvis en ikke-nøgle attribut afhænger af en delmængde af en kandidatnøgle, skal tabellen opdeles).
 
**Hvorfor denormalisere?**  
Forbedre læsehastighed: Mindsker behovet for komplekse joins, hvilket kan give hurtigere forespørgsler.  
Reducerer beregningskrav: Nogle beregninger kan forudlagres i en denormaliseret tabel for at spare tid.  
Optimering til rapportering: Datawarehouse-systemer bruger ofte denormalisering for at forbedre performance i analytiske forespørgsler.
 
**Denormalization:**  
Denormalisering er processen med bevidst at bryde normaliseringsreglerne for at forbedre ydeevnen i en database. Selvom normalisering reducerer redundans og forbedrer dataintegritet, kan det føre til mange join-operationer, hvilket kan gøre forespørgsler langsomme.
 ![Exported image](Exported%20image%2020251104231016-12.png)

**3. Normalform (3NF)**  
Skal opfylde 2NF.  
Ingen ikke-nøgle attribut må afhænge af en anden ikke-nøgle attribut:  
En værdi må ikke være afhængig af en anden, nedenunder vil man kunne ændre LunchPrice til noget andet end halvdelen af Price.  
Eksempel:
 
**2. Normalform (2NF)**  
Skal opfylde 1NF.  
Alle ikke-nøgle attributter skal være fuldstændigt funktionelt afhængige af hele primærnøglen (ingen delvise afhængigheder i sammensatte nøgler).
 
**1. Normalform (1NF)**  
Alle attributter skal være enkelt værdier (ingen lister eller arrays i en enkelt kolonne).  
Hver række skal have en unik identifikator (primær nøgle).  
Der må ikke være duplikerede rækker og kolonner.
 
Normaliseringsformer i SQL-databaser bruges til at organisere data for at reducere redundans og forbedre dataintegritet. Her er en kort beskrivelse af de vigtigste normalformer:
 
## [Normaliserings Former:](https://www.linkedin.com/learning/programming-foundations-databases-2/normalization-2?u=57075649)
   

- UpperCamelCase
- Undgå mellemrum
- Kun på engelsk

**Kolonnenavnet skrives:**

- Ental
- Altid UPPERCASE
- Kun på engelsk
- Brug underscore hvis det er flere ord sat sammen

**Relationsnavnet skrives:**
 
**Navngivnings konventioner:**
 
## Relations skemaer:
      

- One to many
    - Her bliver en værdi fra tabel 1 knyttet til flere i tabel 2
    - Syntax for at skrive det i relations skema:
        - CUSTOMERS(CustomerID, FirstName, LastName, Phone, Birthday) Primær nøglen understreges og skrives først.
        - ORDERS(OrderID, Order, _CustomerID_) Den fremmede nøgle til at kæde tabellerne sammen med skrives med rød og i kursiv.
          
        
- ![Exported image](Exported%20image%2020251104231007-9.png)
- Many to many
    - Her bliver værdier fra tabel 1 knyttet sammen med værdier fra tabel 2 igennem en linking tabel
    - Link relationen skal indeholde en kopi af primær nøglen fra begge tabeller
    - Oftest dannes navnet for relationen ved at sammensætte de 2 tabellers navne, herunder "CustomersDishes".
    - Syntax for at skrive det i relations skema:
        - CUSTOMERS_DISHSES(CustomersID, DishID) Da begge nøgler er primær nøgler, understreges de også.
- ![Exported image](Exported%20image%2020251104231013-10.png)
- One to one
    - Her forbinder man kun en værdi fra den ene tabel med den anden. Denne er ikke særligt udbredt, men stadig relevant
    - Her kan man frit vælge om primær nøglen skal komme fra den ene eller anden tabel.
    - Syntax for at skrive det i relations skema:
        - CUSTOMERS(CustomerID, FirstName, LastName, FavoriteDish)
        - SPECIALREQUESTS(RequestNotes, CustomerID)
      
    
- ![Exported image](Exported%20image%2020251104231014-11.png)

For at beskrive sammenhæng mellem flere tabeller bruger man termerne:
 
## Relationer:
 
Her bruger man kombinationen af LastName og Phone for at få en så unik kombination som muligt. Man kan også bruge flere kolonner til at lave Composite keys.
 ![Exported image](Exported%20image%2020251104231005-8.png)

I nogle tilfælde har man måske ikke mulighed for at tilføje til tabellerne og vil være nødt til at bruge en sammensat/Composite key.

### Composite Key eksempel:
 
Når man selv skaber den værdi man bruger som key kalder man det også for "synthetic" eller "surrogate" key.
 ![Exported image](Exported%20image%2020251104231003-7.png)

I dette tilfælde bruger man CustomerID som Primary key:￼

### Primary Key eksempel:
 
- En kolonne eller et sæt af kolonner, der fungere som en primærnøgle, fordi de entydigt identificerer en række.

**Kandidatnøgler/Canditate Keys:**

- En primærnøgle fra en tabel, der refereres i en anden tabel for at etablere en relation mellem de to tabeller.
- Angives ved at understrege og skrive med rød skrift: CustomerID

**Fremmednøgler/Foreign Keys:**

- Når en enkelt kolonne ikke er tilstrækkelig til entydigt at identificere en række, kan en kombination af to eller flere kolonner bruges som en sammensat nøgle.

**Sammensatte nøgler/Composite Keys:**

- En primærnøgle er en unik værdi, der bruges til at identificere en specifik række i en tabel og gør datahentning mere effektiv.
- Angives ved at understrege nøglen: CustomerID

**Primærnøgler/Primary Keys:**

- Dette er værdier, der ikke gentages i nogen anden række i en given kolonne, hvilket sikrer, at hvert felt er unik.

**Unikke værdier/Unique Keys:**

## Nøgler/Keys:
   

Hver kolonne vil være et sæt af værdier af en specifik type.  
Hver række vil være en entitet af et samlet sæt værdier.  
Relational Database Management System = R-DMS
 ![Exported image](Exported%20image%2020251104231001-6.png)   
## Relational database:
 
Vil man ignorer dele af et statement kan bruge /* */:  
SELECT CustomerName, /*City,*/ Country FROM Customers;
 
```
Flere linje kommentarer skrives med /* indsæt kommentar her */
```
 
```
Enkelt linje kommentarer skrives med to bindestreger:  
-- Select all: \<---- Kommentar  
SELECT * FROM Customers;
```

## Kommentar:
 
Dette sletter hele tabellen.

- DROP TABLE table_name;

Vil man slette alt hele tabellen med alt struktur bruge man:
 
```
Dette sletter indholdet, men beholder strukturen med kolonner og andet.
```

```
DELETE FROM table_name;
```

```
TRUNCATE TABLE table_name;
```

Vil man slette alt indholdet i en tabel kan man bruge:
 ![Exported image](Exported%20image%2020251104230959-5.png)

```
EKSTREMT VIGTIGT!! Husk et WHERE statement der bestemmer hvad der skal slettes. Ellers slettes hele tabellen
```

```
DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';
```

```
Vil man slette en entitet fra en tabel bruger man:
```

## Slet:
 
```
INSERT INTO Customers (CustomerName, City, Country)
```

```
VALUES ('Cardinal', 'Stavanger', 'Norway');
```
 
```
Man vil også kunne nøjes med at udfylde nogle af felterne. De ikke udfyldte vil blive sat i som 'Null':
```

## Indsæt med nogle værdier udeladt:
 
```
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
```

```
VALUES  
('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway'),  
('Greasy Burger', 'Per Olsen', 'Gateveien 15', 'Sandnes', '4306', 'Norway'),  
('Tasty Tee', 'Finn Egan', 'Streetroad 19B', 'Liverpool', 'L1 0AA', 'UK');
```
 
Vil man indsætte flere entiteter ind på engang kan det gøres i samme INSERT INTO statement:

## Indsæt flere:

**Bemærk at man ikke angiver Personid når IDENTITY er oprettet.**
 
```
INSERT INTO Persons (FirstName, LastName, Age)
```

```
VALUES ('Lars', 'Monsen', '32');
```
 
For at indsætte entiteter ind i tabellen bruger man:

## Indsæt:

Her tilføjer man INDENTITY(1,1) til Personid som vil starte på 1 og lægge 1 til for hver gang man tilføjer en ny til listen. Ønsker man at starte på et andet tal end 1 vil man i stedet skrive (10,1) for at starte på 10 f.eks.
 
```
CREATE TABLE Persons (  
    Personid int IDENTITY(1,1) PRIMARY KEY,  
    LastName varchar(255) NOT NULL,  
    FirstName varchar(255),  
    Age int  
);
```
 
```
CREATE TABLE table_name (  
    column1 datatype,  
    column2 datatype,  
    column3 datatype,  
   ....  
);
```

## Syntax til at oprette tabeller:
 ![Exported image](Exported%20image%2020251104230958-4.png)

## Syntax til variabler:
 
Databaser generelt gør det nemt at arbejde med store mængder data. Man vil nemt kunne sortere efter hvad man vil vide og er relevant.
       ![Exported image](Exported%20image%2020251104230957-3.png)

```
{   
"ConnectionStrings": {   
"MyDBConnection":   
"Server=localhost\\SQLEXPRESS;Database=SomeDatabase;Trusted_Connection=True;TrustServerCertificate=true;"  
 }  
}
```

![Exported image](Exported%20image%2020251104230952-2.png)  
![Exported image](Exported%20image%2020251104230951-1.png)  
![Exported image](Exported%20image%2020251104230949-0.png)