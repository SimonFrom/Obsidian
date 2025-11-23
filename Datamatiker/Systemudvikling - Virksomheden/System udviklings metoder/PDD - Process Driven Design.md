## **Hvad er Process-Driven Design?**

**Process-Driven Design** er en tilgang til systemudvikling, hvor **forretningsprocesser** står i centrum. Systemet designes ud fra hvordan arbejdet _faktisk_ udføres i organisationen, fremfor at starte med datastrukturer eller tekniske løsninger.

---

## **Kerneprincipper**

### **1. Processen kommer først**

- Start med at forstå forretningsprocesserne
- Systemet skal understøtte processerne, ikke omvendt
- Processer definerer systemets struktur

### **2. End-to-end perspektiv**

- Fokus på hele arbejdsgangen fra start til slut
- Inkluderer alle involverede aktører og systemer
- Optimerer hele processen, ikke kun dele

### **3. Brugerorienteret**

- Designer ud fra hvordan brugere arbejder
- Minimerer afbrydelser i arbejdsflowet
- Understøtter naturlige arbejdsmønstre

---

## **Centrale Elementer**

### **Forretningsprocesser**

- **Aktiviteter** - Enkelte arbejdsopgaver
- **Beslutningspunkter** - Hvor valg træffes
- **Aktører** - Hvem gør hvad
- **Input/Output** - Data der bruges/produceres
- **Regler** - Hvordan processer styres

### **Procestyper**



- **Kerneprocesser** - Skaber værdi for kunden
- **Støtteprocesser** - Understøtter kerneprocesser
- **Styringsprocesser** - Planlægning og kontrol

---

## **Process-Driven Design Proces**



### **1. Process Discovery (Procesafdækning)**



**Hvad:**

- Identificer og dokumentér eksisterende processer
- Interview medarbejdere og observér arbejdet
- Kortlæg hele procesflowet

**Teknikker:**

- Interviews
- Observation
- Process workshops
- Dokumentanalyse

**Output:**

- As-Is procesdiagrammer
- Procesbeskrivelser
- Pain points og flaskehalse

---

### **2. Process Analysis (Procesanalyse)**



**Hvad:**

- Analysér proceseffektivitet
- Identificér problemer og forbedringspotentiale
- Prioritér indsatsområder

**Fokusområder:**

- ⏱️ Tidsforbrug
- 💰 Omkostninger
- ⚠️ Fejlrate
- 🔄 Gentagelse/redundans
- 🚧 Flaskehalse

**Output:**

- Gap-analyse
- Forbedringsforslag
- Prioriteret liste

---

### **3. Process Design (Procesdesign)**


**Hvad:**

- Design optimerede processer (To-Be)
- Fjern spild og ineffektivitet
- Standardisér hvor muligt

**Principper:**

- Eliminér unødvendige trin
- Automatisér gentagne opgaver
- Parallelisér hvor muligt
- Reducer ventetid
- Simplificér beslutningslogik

**Output:**

- To-Be procesdiagrammer
- Nye standarder
- Automatiseringsmuligheder

---

### **4. System Design (Systemdesign)**


**Hvad:**

- Design IT-system der understøtter processerne
- Systemet følger procesflowet
- Automatisér hvor det giver værdi

**Fokus:**

- Workflows der matcher processer
- Automatiske notifikationer
- Integrationer mellem systemer
- Brugervenlige interfaces

**Output:**

- Systemarkitektur
- User interface designs
- Integration specifications

---

### **5. Implementation (Implementering)**


**Hvad:**

- Implementér nye processer og systemer
- Train medarbejdere
- Monitér og justér

**Aktiviteter:**

- Systemudvikling
- Change management
- Træning
- Pilot-test
- Rollout

---

### **6. Continuous Improvement (Løbende forbedring)**


**Hvad:**

- Monitér procesperformance
- Identificér nye forbedringsmuligheder
- Iterér og optimér

**Metrics:**

- Processtid
- Gennemløbstid
- Fejlrate
- Kundetilfredshed
- Omkostninger pr. proces

---

## **Modelleringsteknikker**


### **1. BPMN (Business Process Model and Notation)**


Standard notation til at modellere forretningsprocesser

**Elementer:**

- ⭕ Events (start, end, intermediate)
- ▭ Activities (opgaver)
- ◇ Gateways (beslutninger)
- → Flow (sekvens)
- 👤 Pools/Lanes (aktører)

**Eksempel:**

```
[Start] → [Modtag ordre] → <Godkend?> → [Ja: Processer] → [Send] → [End]
                              ↓ Nej
                           [Afvis] → [End]
```

---

### **2. UML Activity Diagrams**

Viser workflow og aktiviteter

---

### **3. Swimlane Diagrams**


Viser hvem der gør hvad i processen

```
Kunde:     [Afgiv ordre] ────────────────→ [Modtag vare]
                    ↓
Salg:              [Behandl ordre] → [Send til lager]
                                          ↓
Lager:                                [Pak vare] → [Send]
```

---

### **4. Value Stream Mapping**


Visualiserer værdistrøm og identificerer spild

---

## **Fordele ved Process-Driven Design**


✅ **Forretningsafstemning** - System matcher faktiske behov  
✅ **Bedre brugeroplevelse** - Flow matcher arbejdsmåde  
✅ **Identificerer ineffektivitet** - Finder spild og flaskehalse  
✅ **End-to-end optimering** - Hele processen forbedres  
✅ **Klarere krav** - Processer definerer funktionalitet  
✅ **Lettere vedligeholdelse** - Procesændringer → systemændringer  
✅ **Målbare resultater** - KPI'er baseret på processer

---

## **Ulemper ved Process-Driven Design**


❌ **Tidskrævende** - Grundig procesanalyse tager tid  
❌ **Resistance to change** - Medarbejdere kan modstå nye processer  
❌ **Kompleksitet** - Mange processer kan være svære at håndtere  
❌ **Over-standardisering** - Kan kvæle kreativitet og fleksibilitet  
❌ **Dokumentationsbyrde** - Mange processer skal dokumenteres  
❌ **Risiko for rigiditet** - Svært at tilpasse hvis processer ændrer sig

---

## **Værktøjer til Process-Driven Design**


### **Modelleringsværktøjer**

- **Bizagi Modeler** - Gratis BPMN-værktøj
- **Lucidchart** - Online diagrammer
- **Microsoft Visio** - Process mapping
- **Draw.io** - Gratis alternative
- **Camunda Modeler** - BPMN med execution

### **BPM Systemer (Business Process Management)**


- **Camunda** - Open source workflow engine
- **Activiti** - BPM platform
- **Pega** - Enterprise BPM
- **Appian** - Low-code BPM
- **ServiceNow** - Workflow automation

### **Process Mining**


- **Celonis** - Process mining platform
- **UiPath Process Mining** - Discover actual processes
- **Signavio** - Process analysis

---

## **Process-Driven vs. Data-Driven Design**

|**Aspekt**|**Process-Driven**|**Data-Driven**|
|---|---|---|
|**Udgangspunkt**|Forretningsprocesser|Datastrukturer|
|**Fokus**|Hvordan arbejde udføres|Hvilke data opbevares|
|**Styrke**|Understøtter workflow|Dataintegritet|
|**Svaghed**|Kan ignorere data-kompleksitet|Kan ignorere bruger-behov|
|**Egnet til**|Workflow-tunge systemer|Data-intensive systemer|

---

## **Process-Driven vs. Domain-Driven Design**


|**Aspekt**|**Process-Driven**|**Domain-Driven**|
|---|---|---|
|**Fokus**|Aktiviteter og flow|Forretningslogik og regler|
|**Modellering**|Procesdiagrammer|Domænemodeller|
|**Kompleksitet**|Lavere|Højere|
|**Best fit**|Procesorienterede systemer|Komplekse domæner|

**Kombination:** Ofte bruges begge tilgange sammen - DDD for domænelogik, PDD for workflows

---

## **Hvornår bruge Process-Driven Design?**


### ✅ **Brug PDD når:**


- Systemet skal understøtte komplekse workflows
- Mange aktører involveret i processer
- Procesoptimering er primært mål
- Compliance og standardisering vigtigt
- End-to-end synlighed nødvendig
- Eksempler: BPM-systemer, workflow engines, case management

### ❌ **Undgå PDD når:**


- Simple CRUD-applikationer
- Meget kompleks forretningslogik
- Processer ændrer sig konstant
- Fleksibilitet vigtigere end standardisering
- Eksempler: Analytiske systemer, reporting tools

---

## **Best Practices**


1. **Start med brugerne** - Forstå hvordan de arbejder
2. **Involvér stakeholders** - Alle relevante parter skal deltage
3. **Visualisér processer** - Brug diagrammer alle kan forstå
4. **Mål performance** - Definer KPI'er for processer
5. **Iterér** - Forbedre kontinuerligt
6. **Balance** - Standardisering vs. fleksibilitet
7. **Automatisér smart** - Ikke alt skal automatiseres
8. **Dokumentér** - Men ikke for meget

---

## **Konklusion**


Process-Driven Design er stærkt når **workflows og processer** er centrale for forretningen. Det sikrer at systemer understøtter den måde folk faktisk arbejder på, og gør det lettere at identificere og eliminere ineffektivitet. Kombineret med andre tilgange som DDD kan det give både effektive processer og solid forretningslogik.