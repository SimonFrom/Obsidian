**Skal sættes uden for main og i namespace!!**
 
[BroCode](https://www.youtube.com/watch?v=3p0OJErAbEI&ab_channel=BroCode)
 
En **enumerated type** (enum) i C# er en særlig værdi-type, der bruges til at definere et sæt navngivne konstante værdier. Hver værdi i en enum er tildelt en underliggende heltalsværdi (som standard starter det fra 0). Det er nyttigt til at repræsentere et begrænset sæt af mulige værdier, som f.eks. ugedage, farver eller statusser.
 ![Exported image](Exported%20image%2020251104230744-0.png)   
**Enums** har flere indbyggede metoder:
 
- **Enum.GetNames()**: Returnerer et array af navnene i en enum.
- **Enum.GetValues()**: Returnerer et array af værdierne i en enum.
- **Enum.IsDefined()**: Tjekker, om en værdi findes i en enum
- **(int)DayOfWeek.Monday** vil returnere 1, da nul indeks er anvendt.