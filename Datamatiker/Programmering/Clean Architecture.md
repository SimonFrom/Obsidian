Ideen med Clean Architecture er at man deler applikationen op i lag med hver deres ansvar.
Derudover er domænet også en central del. Alle dele ender i domænet.

Det primære mål er at opnå:
1. Maintainable
	- Nemt at forstå og ændre, minimal risiko for at indføre bugs.
2. Testable:
	- På grund af opdelingen er det nemt at teste dele af applikationen uafhængigt af hindanden. 
3. Independent of frameworks:
	- Med opdelingen er det også nemmere at skifte framework, da hver del kan se som en selvstændig komponent.
4. Independent of UI:
	- UI har ingen afhængighed af logikken eller regelsæt opsat senere i koden, nemt at lave cross platform.
5. Independent of database:
	- Det er nemt at skifte en database type ud med en anden, da persistens laget er lavet med interface og abstrakte metoder.
6. Independent of any external agency:


## Lagene i Clean Architecture:

### Domain:
- Entiteter
- Domænelogik
- Forretningslogik
### Application:
- Services
### Infrastructure:
- Database adgang/Entity Framework
### Presentation:
- UI/Web UI

### Solution struktur:
![[Pasted image 20251126125627.png]]

Ideen er at man tilføjer referencer tilbage til det foregående ClassLibrary.
WebUI --> Infrastructure --> Domain --> Application

I hvert projekt laver man en Dependency Injection klasse til som man til sidst injecter i WebUI.Server projektet. Denne skal indeholde de repositories, interfaces, modeller eller hvad man skal bruge som man normalt ville registrere i program direkte.