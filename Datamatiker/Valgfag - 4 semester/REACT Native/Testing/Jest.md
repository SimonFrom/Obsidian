Jest er et Javascript framework til at teste f.eks React Native applikationer. 

Et eksempel kunne være: 
```
it('given a date in the past, colorForDueDate() returns red', () => {  

expect(colorForDueDate('2000-10-20')).toBe('red');  

});
```

Selve it funktionen tager blandt andet imod en streng til at beskrive hvad der bliver testet. Her testes der om en metode reagerer korrekt når der bliver givet en dato i fortiden.

En god huske regel er at dække følgende:
1. Given - Input
2. When - Hvor inputtet skal bruges
3. Then - Hvad outputtet bør være

Det kan sammenlignes med det vi kender fra Arrange, Act, Assert mønstret.

Unit testing bliver også brugt

## Mocking:
Hvis ens app har eksterne afhængigheder er det en god idé at *mocke* dem for at undgå at testen fejler på grund af noget som er uden for ens kontrol, et api der går offline f.eks.

Så i stedet for at kalde api'et vil man bare lave et objekt der simulerer det svar man ville få.

## Komponent test:
Det er også vigtigt at teste ens komponenter separat fra logikken. Komponenten kan sagtens vise noget forkert, ikke reagere eller andre uhensigtsmæssige ting, på trods af at logikken gør som man forventer. 
I teorien kunne man beskrive komponent tests som en del af Unit eller Integrations tests.

Det er vigtigt at huske at disse tests kun fungerer i et JavaScript Node.js miljø. Så de vil ikke give en 100% afklaring på om det hele fungerer som tiltænkt på ios og android enheder.

Generalt skal man skrive testen fra brugerens synspunkt og hvordan de ser app'en. Så man skal ikke tænke så meget på den hårde logik, denne burde også testes seperat før, så man kan sikre at det komponenten modtager korrekt data.