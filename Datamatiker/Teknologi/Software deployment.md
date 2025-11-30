Dette indebærer den proces det er at flytte kode og programmer fra et miljø til et andet miljø. Typisk fra **udvikling** til **staging** eller **staging** til **production**

#### Faser i deployment:
1. **Development:**
	- Deployment begynder allerede i udviklings fasen. Planlægning og versions styring er vigtige ting her.
2. **Testing and QA:**
	- Alt skal testes inden det sendes videre i deployment rækkefølgen.
	- **Automatiserede tests**: Unit test og integrations test verificer at koden virker som den er tiltænkt uden brugere.
	- **Manuelle tests**: QA testere undersøger og tester softwaren som brugere ville bruge den. Dette sørger for at programmet passer til målgruppens behov og evner.
3. **Staging environment:**
	- En replica af et production miljø til at simulere en rigtig release. Her kan man sikre at programmet vil fungerer korrekt i brugerenes miljø.
4. **Production deployment:**
	- Udgivelse til brugere og kunder. 
	- **Timing:** Tag hensyn til bruger hvis systemet er nød til at være offline for opdatering.
	- **Access Control**: Sørg for at kun de rette brugere kan godkende opdateringer.
	- **Info**: Sørg for at alle stakeholders der er berørte af opdateringen er informeret.
5. **Monitoring and maintenance:**
	- Man bør overvåge og holde øje med systemet efter release for at sikre at det kører optimalt eller kan fange bugs i opløbet inden der sker alvorlige fejl.

#### Strategier:
1. **Blue-Green deployment**
Her kører man med to produktions miljøer samtidigt. 
Blå = gammelt og grøn = nyt.
Man deployer til det inaktive/nye miljø først og tester, for derefter at flytte trafikken over. Går der noget galt vil man have et stabilt miljø at flytte tilbage i.
2. **Canary deployment**
Her deployer man til en lille gruppe ad gangen og fortsætter indtil alle er med.
3. **Rolling deployment**
Man opdaterer forskellige ting en af gangen. En server eller container af gangen.

#### Værktøjer:
1. **Continuous integration/continuous delivery (CI/CD)**
	- Når der bliver committet noget til et repo vil disse værktøjer sørger for at bygge og deploye kode hver gang.
2. **Configuration management**:
	- Hjælper med at sørge for at hvert miljø har de samme forudsætninger og indstillinger.
3. **Containerization**
4. **Monitoring**

#### Best practices:
- Automatiser alt man kan.
- Test grundigt.
- Planlæg rollbacks.
- Sørg for at koden ikke indeholder miljø specifikke settings.
- Brug værktøjer til at holde hvad der er sendt ud.