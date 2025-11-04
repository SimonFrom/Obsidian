## Hvad er unit testing?

En Unit Test er en test af et stykke kode, ofte metoder.  
Stykket man tester bliver kaldt en Unit.  
Testen er et stykke kode som oftest er skrevet som funktioner og designet til at analysere om resultatet af Unit Testen er som det forventede.  
Hoved formålet er isolere et stykke kode og bestemme dens korrekthed.  
**Man må** **ikke** **ændre i en unit test uden at have aftalt det med programøreren der har skrevet den!!**
 
## Hvorfor er det nødvendigt?

Unit Testing kan øge sikkerheden for at et stykke kode fungerer 100% som tiltænkt og derved kan man tillade sig bruge det flere steder uden bekymring for bugs og fejl.
 
## TDD(Test Driven Development)

TDD er et af grund principperne i Unit Test. Her vil man skrive testen der skal bestås først og derefter skriver man et stykke kode der løser testen.
 
## AAA Mønster:

Man følger gerne et Act, Arrange og Assert mønster.

- Arrange:
    - Arrange-fasen forbereder du alt, hvad du skal bruge til testen. Du opretter objekter, initialiserer variabler og konfigurerer den nødvendige tilstand for at gennemføre testen.
- Act:
    - I Act-fasen udfører du den handling, du tester. Dette kunne være at kalde en metode, opdatere en tilstand eller interagere med en klasse.
- Assert:
    - I Assert-fasen kontrollerer du, om resultaterne af handlingen (fra Act) er som forventet. Du sammenligner den faktiske output med den forventede værdi og bekræfter, at de stemmer overens.
 
## Krav til syntax og klasse

Hver test metode man skriver skal have TestMethod øverst.  
Metodens return skal være Void.  
Der må ikke være nogle parametre i metoden.  
Eksempel:

![TestMethod public void Test_AddMethod new BasicMat...](Exported%20image%2020251104230525-0.png)  

[Unit Testing](https://www.c-sharpcorner.com/article/a-basic-introduction-of-unit-test-for-beginners/)