[[High level design]]
- I en use case skal der beskrives en situation og beskrives hvordan et system skal agere. Man skal ikke beskrive i detaljer hvordan systemet fungerer.
- Der vil være en aktør/bruger som sender inputs ind i systemet og systemet sender noget tilbage.
- Beskrivelsen, navngivningen på aktøren og titlen på casen skal afspejle det forretningsmæssige formål.
- I en Use Case har man også feature fokus. Altså fokuser på funktionerne og ikke user story  
# Aktøren

Use casen skal også fortælle, hvilke aktører der udfører brugssituationen. En aktør kan sammenlignes med en arbejdsrolle i virksomheden, som for eksempel en bogholder eller en sælger. Aktøren kan også være et andet system eller en organisation.  
Aktører kan deles op i tre grupper.

- Primary actor· er den bruger(aktør), der skal bruge systemet for at få sit arbejde udført. Når du har fundet primary actors, kan du også finde de behov, som din use case skal opfylde.
- Supporting actor · er en, der yder en service til systemet. Det kan for eksempel være en ekstern service, som udfører en del af det, use casen skal kunne. Når du har fundet supporting actors, kan du få klarlagt behovet for eksterne interfaces og protokoller.
- Offstage actors · er interessenter i use casen, men som ikke er primary eller supporting. Dette kan for eksempel være myndigheder, som skal bruge data fra systemet. Eller det kan være kunder, som ikke er i direkte kontakt med systemet, men som leverer data, som skal bruges i systemet. Disse er vigtige at finde, da de er lette at overse, og de kan have krav til systemet, som SKAL overholdes.

# Detaljeringsgraden på en use case

En use case kan enten være brief, casual eller Fully dressed . Disse betegnelser dækker over detaljerings-graden af en use case.

- **Brief** har kun et enkelt afsnit, der beskriver hvordan systemet skal agere, hvis alt går ideelt.
- **Casual** har flere afsnit og beskriver også alternative måder at agere på i forhold til forskellige udfald.
- **Fully** **dressed** beskriver alle trin, variationer og krav i detaljer, og der er også angivet pre- og post-conditions.
![Process Sale A customer arrives at a checkout with...](Exported%20image%2020251104231114-0.png)

Eksempel på en Use Case fra Larman.
 
Det er vigtigt at beskrive Use Casen/brugssituationen på en så general måde som muligt så den kan dække over flere forskellige situationer og scenarier.
    

# Brug Computational Thinking

I arbejdet med at få skrevet gode use cases skal man kunne forskellige ting. I Computational Thinking indgår fire elementer, som I kan gøre brug af her:

- Man skal kunne forstå forskellige brugere og de forskellige situationer og måder, de kommer til at bruge systemet på (decomposition). F.eks. at der er forskellige betalingsmåder.
- Man skal kunne sortere i informationerne, så man kun får det relevante med (abstraction). F.eks. er det sandsynligvis ikke relevant for systemet og dets brug, at Ib vil spise sine bananer til frokost.
- Man skal kunne genkende, det der går igen (pattern recognition). F.eks. at selv om der betales på forskellige måder, så betaler alle dog.
- Og man skal kunne skille de enkelte trin fra hinanden, og kunne se sammenhængen (algorithms). F.eks. først kommer kunden med varerne, så betales der, så udleveres bon´en.
 
# Use Case test

- **Boss**: enkel metode til at vurdere, om en use case er korrekt defineret og fokuserer på brugerens interaktion med systemet på en meningsfuld måde
- **EBP** (Elementary Business Process): bruges til at evaluere en use case for at sikre, at den beskriver en fuldstændig og meningsfuld forretningsproces på det rette niveau. Den fokuserer på at validere, om use casen er tilstrækkelig detaljeret og værdifuld for brugeren og forretningsmålet.
- **Size**: Size-testen bruges i forbindelse med use cases til at vurdere, om en use case er på det rette detaljeniveau. Testen sikrer, at en use case hverken er for stor (overordnet) eller for lille (teknisk eller fragmenteret), men snarere balanceret og håndterbar.
   

Stakeholders and Interests  
Overskriften refererer til alle der er i kontakt med systemet på den ene eller anden måde.  
I et POS kunne det eksempelvis være:

![Cashier Wants accurate fast entry and no payment e...](Exported%20image%2020251104231117-1.png) ![615 Guideline How to Find Use Cases Use cases are ...](Exported%20image%2020251104231119-2.png)

Læs evt op i Larman, kapitel 6, side 68.

![Exported image](Exported%20image%2020251104231121-3.png)