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

Generelt skal man skrive testen fra brugerens synspunkt og hvordan de ser app'en. Så man skal ikke tænke så meget på den hårde logik, denne burde også testes seperat før, så man kan sikre at det komponenten modtager korrekt data.

## Simuler bruger input:
For at kunne teste en brugers input uden at fysisk skulle trykke på knapperne, kan man bruge tre metoder fra [React Native Testing Library](https://oss.callstack.com/react-native-testing-library/). 
- fireEvent - Aktiverer et event, onChangeText f.eks
```javascript
fireEvent.changeText(  
getByPlaceholderText('Enter grocery item'),  
'banana',  
);
```
- press - Laver et tryk på en knap f.eks
```javascript
fireEvent.press(getByText('Add the item to list'));
```
- getAllByText(text, options?) - Søger i det renderede DOM og returnere alle elementer der passer. Man kan præcisere hvad med søger efter med selector options, f.eks getAllByText('banana', { selector: 'button' })
```javascript
const bananaElements = getAllByText('banana');  
expect(bananaElements).toHaveLength(1);
```

Det overstående forløb skulle gerne:
1. Aktivere et changeText event med strengen 'banana' i et tekstfelt
2. "Trykke" på knappen til at tilføje til en liste
3. Finde alle elementer med 'banana' og tjekke om listen indeholder 'banana'

## Snapshot testing:
Det er også muligt at teste ens komponenters layout og design. 
Et komponent snapshot kan forklares som en nemmere læsbar udgave af en tsx/jsx komponent, som bliver genereret under en test. 

```typescript
<Text
  style={
    Object {
      "fontSize": 20,
      "textAlign": "center",
    }
  }>
  Welcome to React Native!
</Text>
```

Som en generel regel er det vigtigt at holde ens snapshots små og kun til enkelt stående komponenter, ikke hele sider, en knap eller tekst felt f.eks. Alt der ikke matcher snapshottet vil få testen til at fejle, så selv meget små ændringer vil give en fejl.

## End-To-End/E2E test:
Ønsker man at teste ens komplette app kan man bruge E2E tests. Her tænker man ikke på komponenter, api'er eller logik. 
Her kan man finde og interagere med elementer på skærmen, teste om elementer rent faktisk renderer, hvad tekst der står og så videre...

[Detox](https://github.com/wix/detox/), [appium](https://appium.io/docs/en/latest/) og [Maestro](https://docs.maestro.dev/) er populære tools til E2E.