[String GPT Chat](https://chatgpt.com/c/677b7edd-9158-8006-a6e6-5d4c44af4527)
    
String er en data type som repræsenterer en sekvens af unicode tegn, altså en række af  
Bogstaver, tal eller symboler.  
Tal bliver ikke gemt som en numerisk værdi, der kan altså ikke laves beregninger med dem. ￼For at kunne dette ville man skulle gør som her:

![Exported image](Exported%20image%2020251104230713-0.png)  

Andre metoder og egenskaber er:
 
- Length: Returnerer længden af strengen.
- ToUpper() og ToLower(): Ændrer bogstaver til store eller små.
- Substring(): Udtrækker en del af en streng.
- Split(): Deler en streng i en array baseret på en separator.
- Replace(): Erstatter en del af strengen med noget andet.
- Trim(): Fjerner mellemrum fra starten og slutningen af en streng.
 
Med String Interpolation($) kan man indsætte en anden datatype i sin string.

![Exported image](Exported%20image%2020251104230714-1.png)  

Kan også bruges til at samle forskellige værdier til en samlet string.

![Exported image](Exported%20image%2020251104230719-2.png)  

Ved store eller mange ændringer bør man bruge StringBuilder klassen i stedet.
 
**Hvad er StringBuilder?**  
StringBuilder er en klasse i System.Text-navnerummet, der bruges til at manipulere tekst  
mere effektivt end den immutable string-datatype.
 
**Immutable strings vs mutable StringBuilder:**

- **string** er immutable, hvilket betyder, at hver gang du ændrer en string,

oprettes en ny instans i hukommelsen. Dette kan være ineffektivt ved mange ændringer.

- **StringBuilder** er mutable (ændringsbar), hvilket gør den ideel til scenarier,

hvor du ofte tilføjer, fjerner eller ændrer tekst.
 
Eksempel:

![Exported image](Exported%20image%2020251104230720-3.png)   
**Insert()**

![Exported image](Exported%20image%2020251104230721-4.png)  

**Replace()**

![Exported image](Exported%20image%2020251104230722-5.png)  

**Remove()**

![Exported image](Exported%20image%2020251104230724-6.png)  
![Exported image](Exported%20image%2020251104230725-7.png)