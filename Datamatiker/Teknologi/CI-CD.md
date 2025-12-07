*CI/CD står for :
"Continuous integration and continuous delivery/development"*

**CI** indebærer uatomatisk og ofte integration af ny kode i et delt source kode repository.
**CD** er en proces med to skridt:
1. Integration og test af kode.
2. Levering af kode.
	1. **Delivery** vil release til et repository.
	2. **Development** vil deploye direkte til produktion.


![[Pasted image 20251207120320.png]]

**CI** handler også omkring håndteringen af flere udviklere der skal arbejde på hver deres del af programmet og når deres features skal merges sammen.

**Continuous Delivery** sørger for at den reviewede kode automatisk bliver testet når det uploades til et repository. Herfra kan et andet team være sikker på at det er klar til brug.

**Continuous Development** tager det et skridt videre og sørger for at koden automatisk bliver deployet til et produktions miljø. 