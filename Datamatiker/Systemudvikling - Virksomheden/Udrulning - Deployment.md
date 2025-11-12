Målet med en udrulnings strategi er at forudse så mange problemer og udfordringer som muligt.
Det kan være:
- Fysiske omgivelser
- Hardware
- Dokumentation
- Træning
- Database
- Tredje parts software
- Ens eget system

Typiske fejl eller forglemmelser i planlægningen kan være:
- At man antager at alt virker. "*Det virkede igår*" citat: samtlige studerende til præsentationen...
- Ingen rollback plan
	- Denne skal altid tage hensyn til alt. Det kan være images af operativ systemer eller databaser. Det skal altid være muligt at gå tilbage til skridtet før.
- For stram deadline
- Ikke erkende hvornår man skal stoppe og udskyde udrulningen til et andet tidspunkt
- Springe staging delen over
- Store ændringer eller opdateringer samtidigt - små ændringer gør det nemmere at spore hvor det gik galt
- Ustabilt hardware

Overordnet kan man sige at jo større scope systemet eller projektet har, jo mere præcis og gennemarbejdet plan skal man have. 



### Cutover
- Processen i at flytte brugere til en ny platform.
- Forskelligt for projekt til projekt hvad der giver mening.
- Målet er at brugerne sådan set ikke mærker skiftet, som ved natlige opdateringer f.eks.

#### Cutover metoder:
- **Staged Deployment:**
	- Hvis man ikke kan reducere omfanget af en katastrofal fejl, kan man nogle gange nedbringe sandsynligheden for dem ved at bruge **Staged Deployment**. Dette sørger for at man bruger et *staging area* til at agere testbænk/legeplads hvor man simulere det rigtige miljø og træne til den rigtige deployment.
	- I takt med at man fjerner flere bugs og får planlagt beredskabet, kan også skrue op for realismen i *staging area*.
	- Ideelt bruger man superbrugere til at teste med da de oftest kender brugernes problemer end udviklerne og de vil også få et større indblik og mere erfaring i programmet inden den fulde release.
- **Gradual Cutover**
	- Her flytter man brugerne over i små grupper og gennem tester det hele, hver gang.
	- En fordel er at man sikrer at der er altid er medarbejdere der kan arbejde produktivt uden at skulle kæmpe med software.
	- En ulempe kan være at der er to systemer der skal supporteres samtidigt.
- **Incremental Cutover:**
	- Her udbyder man systemet i små bidder af gangen, eventuelt samtidig med **Staged** eller **Gradual**. 
	- Bedst til mindre systemer, da større *Monolithic applications* tit er afhængige af sig selv i stor grad.
	- Passer godt sammen med iterative udviklings modeller, såsom RAD, da man udvikler små bider af gangen i forvejen.
- **Parallel Testing:**
	- Her tester slutbrugerne systemet med mock data eller trænings scenarier, samtidig med at andre arbejder videre i det gamle og holder forretningen kørende.
	- Når man føler at systemet lever op til kravene kan man bruge **Staged** eller **Gradual** igen for at gøre skiftet blødt.
