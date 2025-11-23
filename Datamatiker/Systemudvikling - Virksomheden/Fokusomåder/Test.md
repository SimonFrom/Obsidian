## Mål med testning

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#m%C3%A5l-med-testning)

De primære mål med testning rækker ud over blot at finde fejl:

- Find defekter før brugerne gør - Fang problemer i kontrollerede miljøer frem for i produktion
    
- Verificer at krav er opfyldt - Sikr at softwaren gør det, den skal
    
- Validér brugerforventninger - Bekræft at softwaren løser rigtige brugerproblemer
    
- Vurdér kvalitet og risiko - Forstå systemets stabilitet og pålidelighed
    
- Forhindre regressioner - Sikr at nye ændringer ikke ødelægger eksisterende funktionalitet
    
- Opbyg tillid - Giv interessenter sikkerhed for at softwaren er klar til levering
    
- Forbedr udviklingsprocessen - Feedback fra testning hjælper med at forfine kodepraksis og arkitektur
    

## Grunde til at du måske ikke vil fjerne en bug

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#grunde-til-at-du-m%C3%A5ske-ikke-vil-fjerne-en-bug)

Modintutivt bør ikke alle bugs rettes med det samme:

- Brugere er afhængige af opførslen - "Buggen" kan være blevet en funktion, man stoler på
    
- Rettelsen er mere risikabel end buggen - Ændringer kan introducere mere alvorlige problemer
    
- Lav indvirkning, høje omkostninger - Indsatsen for at rette opvejer ikke den minimale brugerindvirkning
    
- Tæt på end-of-life - Produktet bliver snart udfaset alligevel
    
- Der findes workarounds - Brugere har brugbare alternativer, der virker
    
- Performance-afvejninger - Rettelsen kan forringe systemets ydeevne uacceptabelt
    
- Breaking API-ændringer - Rettelsen ville bryde bagudkompatibilitet for integratorer
    
- Kun edge case - Påvirker et så sjældent scenarie, at ressourcer er bedre brugt andre steder
    
- Kompleks funktionsinteraktion - Buggen er dybt sammenflettet med andre systemer
    

## Hvordan man prioriterer bugs

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#hvordan-man-prioriterer-bugs)

Effektiv bug-prioritering overvejer typisk flere faktorer:

Alvorlighed × Hyppighed × Synlighed = Prioritet

Nøgledimensioner at evaluere:

- Indvirkning på brugere - Blokerer det kritiske workflows? Forårsager datatab? Skaber sikkerhedssårbarheder?
    
- Forekomstfrekvens - Hvor ofte vil brugere støde på dette?
    
- Antal berørte brugere - Er det udbredt eller niche?
    
- Tilgængelighed af workaround - Kan brugere opnå deres mål på en anden måde?
    
- Forretningsmæssig indvirkning - Påvirker det omsætning, omdømme eller overholdelse?
    
- Kompleksitet og risiko ved rettelse - Kan vi rette det sikkert og hurtigt?
    

Almindelige prioritetsrammer:

- P0/Kritisk: Blokerer releases, forårsager datatab, sikkerhedsproblemer
    
- P1/Høj: Større funktionalitet ødelagt, påvirker mange brugere
    
- P2/Mellem: Moderat indvirkning, har workarounds
    
- P3/Lav: Mindre problemer, kosmetiske fejl
    

## Slags tests og testteknikker

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#slags-tests-og-testteknikker)

### Typer efter omfang

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#typer-efter-omfang)

Unit Testing - Test individuelle komponenter isoleret med mockede afhængigheder

Integrationstest - Verificer at flere komponenter fungerer korrekt sammen

Systemtest - Test det komplette integrerede system mod krav

End-to-End Test - Validér hele workflows fra brugerperspektiv

Accepttest - Bekræft at systemet opfylder forretningskrav og er klar til deployment

**

- Unit test : Verificering
    
- Integration test : Verificering
    
- Regression test : Verificering
    

- Automated testing: Verificering
    
- Component interface test: Verificering
    
- System testing: Verificering
    
- Acceptance testing : Validering
    

**

|**Unit test**| Tester individuelle kodeenheder (funktioner, metoder) mod deres design.|✅ **Verificerende**|Sikrer, at koden virker som specificeret.|

|**Integration test**|Tester samspillet mellem moduler eller komponenter.|✅ **Verificerende**|Tjekker at modulerne samarbejder korrekt ift. design.|

|**Regression test**|Tester at eksisterende funktionalitet stadig virker efter ændringer.|✅ **Verificerende**|Bekræfter at systemet fortsat lever op til tidligere specifikationer.|

|**Automated testing**|En metode (ikke en testtype i sig selv), men bruges typisk til verificerende tests (unit, integration, regression).|✅ **Oftest verificerende**|Automatisering af verificerende testprocesser.|

|**Component interface test**|Tester grænsefladerne mellem komponenter.|✅ **Verificerende**|Tjekker at input/output passer til specifikationerne.|

|**System testing**|Tester hele systemet samlet mod kravspecifikationen.|⚙️ **Primært verificerende**, men kan have validerende elementer|Kontrollerer, at systemet lever op til kravene — stadig mest teknisk.|

|**Acceptance testing**|Tester om systemet opfylder forretningsmæssige krav og brugernes behov.|💡 **Validerende**|Bekræfter, at systemet gør det, kunden faktisk ønsker.|

### Typer efter tilgang

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#typer-efter-tilgang)

Funktionel testning - Gør det, hvad det skal?

- Black box testing (testning uden kendskab til intern kode)
    
- White box testing (testning med kendskab til implementering)
    
- Gray box testing (delvist kendskab)
    

Ikke-funktionel testning

- Performance-test (load, stress, spike, udholdenhed)
    
- Sikkerhedstest (penetration, sårbarhedsscanning)
    
- Brugbarhedstest
    
- Kompatibilitetstest (browsere, enheder, OS)
    
- Tilgængelighedstest
    

Regressionstest - Sikr at eksisterende funktionalitet stadig virker efter ændringer

Smoke Testing - Hurtig verificering af at kritiske funktioner virker

Eksplorativ testning - Uscriptet undersøgelse for at opdage uventede problemer

### Testteknikker

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#testteknikker)

Ækvivalenspartitionering - Opdel input i grupper, der bør opføre sig ens

Boundary Value Analysis - Test ved kanterne af gyldige inputområder

Decision Table Testing - Test kombinationer af betingelser systematisk

State Transition Testing - Verificer tilstandsændringer i systemet

Error Guessing - Brug erfaring til at forudse hvor bugs kan gemme sig

Property-Based Testing - Generér tilfældige input, der opfylder egenskaber

Mutation Testing - Introducér bugs for at verificere at tests fanger dem

Fuzz Testing - Brug tilfældige/ugyldige input for at finde crashes og sikkerhedsproblemer

## Gode testvaner

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#gode-testvaner)

For testdesign:

- Skriv tests før eller sammen med kode (Test-Driven Development)
    
- Test én ting pr. test - hold tests fokuserede og atomare
    
- Brug beskrivende testnavne, der forklarer hvad der testes
    
- Følg Arrange-Act-Assert-mønsteret
    
- Test både happy paths og fejltilfælde
    
- Test ikke kun hvad der virker - test hvad der bør fejle
    

For testvedligeholdelse:

- Hold tests uafhængige - ingen test bør afhænge af en anden
    
- Undgå skrøbelige tests, der går i stykker ved mindre UI-ændringer
    
- Gør tests hurtige, så folk rent faktisk kører dem
    
- Fjern eller ret flaky tests med det samme
    
- Refaktorér tests, når du refaktorerer kode
    
- Brug test fixtures og factories for at reducere duplikering
    

For testdækning:

- Stræb efter høj dækning på kritiske paths, ikke 100% overalt
    
- Fokusér på forretningslogik frem for boilerplate
    
- Test edge cases og grænsetilstande
    
- Inkludér tests for rapporterede bugs for at forhindre regressioner
    
- Test fejlhåndtering og exception paths
    

Kulturelle vaner:

- Behandl testkode med samme omhu som produktionskode
    
- Gennemgå tests i code reviews
    
- Kør tests hyppigt (CI/CD-integration)
    
- Commit ikke kode, der ødelægger tests
    
- Fejr når tests fanger bugs
    
- Del interessante bugs og deres tests med teamet
    

## Metoder til at estimere antal bugs

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#metoder-til-at-estimere-antal-bugs)

At estimere tilbageværende bugs er i sagens natur usikkert, men der findes flere tilgange:

### Statistiske metoder

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#statistiske-metoder)

Capture-Recapture-metoden - Lad to uafhængige teams finde bugs. Overlap hjælper med at estimere totale bugs.

- Formel: Totale Bugs ≈ (Bugs fundet af Team A × Bugs fundet af Team B) / Bugs fundet af begge

Error Seeding - Indsæt bevidst kendte bugs, se derefter hvilken procent testere finder

- Hvis testere finder 80% af seedede bugs, har de sandsynligvis fundet ~80% af rigtige bugs

Bug Discovery Rate-analyse - Plot bugs fundet over tid. Når kurven flader ud, nærmer du dig totalen.

### Empiriske metoder

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#empiriske-metoder)

Historiske data - Brug bug-densitet fra lignende tidligere projekter

- Bugs pr. KLOC (tusinde linjer kode)
    
- Bugs pr. funktionspoint
    
- Bugs pr. feature
    

Kodekompleksitetsmålinger - Mere kompleks kode har tendens til at have flere bugs

- Cyklomatisk kompleksitet
    
- Coupling og cohesion-målinger
    
- Code churn rates
    

Defektforudsigelsesmodeller - Machine learning-modeller trænet på historiske data

### Praktiske indikatorer

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#praktiske-indikatorer)

Diminishing Returns - Når testere kæmper for at finde nye bugs, er du tæt på

Testdækning - Højere kode/path-dækning antyder færre skjulte bugs (men er ingen garanti)

Alvorlighedsfordeling - At finde kun lavt-alvorlige bugs antyder, at store er fundet

Uafhængige teamresultater - Lad et nyt team teste - hvis de finder få nye bugs, er du godt kørende

### Tommelfingerregler

[](https://github.com/bofl2002/Obsidian/blob/main/Testing.md#tommelfingerregler)

Branchegennemsnit antyder:

- Initial udvikling: 15-50 bugs pr. 1000 linjer kode
    
- Efter unit testing: 5-15 bugs pr. KLOC
    
- Efter systemtest: 0,5-3 bugs pr. KLOC
    
- Released software: 0,1-1 bugs pr. KLOC
    

Disse varierer enormt efter domæne - rumfart og medicinsk software har meget lavere rates, mens hurtige forbruger-apps kan have højere rates.

Vigtig advarsel: Disse metoder giver i bedste fald grove estimater. De er nyttige til planlægning og ressourceallokering, men bør ikke behandles som præcise forudsigelser. Målet er ikke at finde hver eneste bug, men at finde bugs der betyder noget, før brugerne gør.

**