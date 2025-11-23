## Gathering requirements

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#gathering-requirements)

### Rich Pictures: Et værktøj til at forstå arbejdskontekst

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#rich-pictures-et-v%C3%A6rkt%C3%B8j-til-at-forst%C3%A5-arbejdskontekst)

Hvad er Rich Pictures? Rich Pictures er tegning-lignende diagrammer, der visualiserer alle interessenter, deres bekymringer og strukturen i en arbejdssammenhæng. De stammer fra Soft Systems Methodology (SSM) og bruges til at forstå komplekse arbejdssituationer før design af nye systemer.

Hovedkomponenter:

- Struktur: Organisationshierarkier, fysisk udstyr, geografiske forhold - ting der ændrer sig langsomt
    
- Processer: Transformationer af varer, dokumenter eller data gennem arbejdsforløbet
    
- Bekymringer: De højniveau-mål og motivationer, der driver forskellige interessenters adfærd
    

Hvordan bruges de? Rich Pictures skabes gennem interviews med interessenter på deres arbejdsplads. De fungerer som et iterativt værktøj, hvor forståelsen raffineres gennem flere runder af tegning og feedback.

Anvendelsesområder:

1. Participatory Design: Hjælper med at identificere alle relevante interessenter og deres perspektiver
    
2. Lightweight Usability Methods: Bruges som første skridt i enkle designprocesser for mindre projekter
    

Fordele:

- Gør komplekse arbejdssituationer synlige
    
- Afslører konflikter mellem forskellige interessenters mål
    
- Billig og hurtig metode, der kræver minimal træning
    
- Kan kombineres med andre analyseteknikker
    

Rich Pictures er altså et praktisk værktøj til at få overblik over "det store billede" før man designer nye systemer.

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md)

## Requirements gathering

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#requirements-gathering)

Krav er de funktioner og egenskaber, som din applikation skal levere. De bruges i tre faser:

1. I starten: Indsamles fra kunder for at forstå, hvad der skal bygges
    
2. Under udvikling: Vejleder udviklingsprocessen og sikrer retningen
    
3. Ved projektets afslutning: Verificerer at det færdige system gør, hvad det skal
    

#### Omfang og formalitet

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#omfang-og-formalitet)

Omfanget varierer drastisk:

- Fra få enkle krav til hundredvis af sider
    
- Afhænger af projektets størrelse, kompleksitet og formalitetsniveau
    

Eksempler på forskellige niveauer:

Uformelle projekter (interne):

- "Find måder at forbedre ordrebehandling"
    
- "Skriv et værktøj til at sende spam til kunder"
    
- Vage krav kan være acceptable, så længe resultatet er nogenlunde brugbart
    

Kritiske systemer:

- Autopilot til Boeing 747
    
- Software til pacemakere
    
- Krav skal være utvetydige og detaljerede
    
- Ingen plads til misforståelser (f.eks. hvad "nem installation" betyder)
    

#### Centrale pointer

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#centrale-pointer)

- Jo højere indsats og risiko, des mere formelle og detaljerede krav
    
- Vage krav kan skabe problemer senere i projektet
    
- Krav skal have specifikke egenskaber for at være nyttige (dette uddybes i de følgende afsnit)
    

### klare krav

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#klare-krav)

Klare krav skal være:

- Koncise og lette at forstå
    
- Fri for "managersprog", blomstrende prosa og forvirrende jargon
    
- Konkrete og utvetydige
    

#### Brug af tekniske termer

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#brug-af-tekniske-termer)

Acceptabelt når:

- Terminologien er defineret et sted
    
- Den er almindelig viden inden for projektets domæne
    

## Entydige krav

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#entydige-krav)

Ud over at være klare og konkrete skal krav også være entydige - der må kun være én mulig at fortolke dem på. Hvis kravet kan forstås på flere måder, kan du ikke bygge et system, der opfylder det.

#### Praktisk eksempel: Ruteplanlægning for rulleskøjteløbere

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#praktisk-eksempel-ruteplanl%C3%A6gning-for-rullesk%C3%B8jtel%C3%B8bere)

Tilsyneladende klart krav: "Programmet skal finde den bedste rute fra start til destination"

Problemet - hvad betyder "bedste"?

- Korteste rute (i afstand)?
    
- Korteste rute (i tid)?
    
- Rute der kun bruger cykelstier (for sikkerhed)?
    
- Rute der passerer flest Starbucks?
    

Yderligere kompleksitet: Selv "korteste" er tvetydigt:

- Korteste i afstand vs. korteste i tid
    
- Hvad hvis den korteste afstand går op ad en stejl bakke og derfor tager længere tid?
    
- Hvad hvis ruten indebærer at gå ned ad trapper?
    

#### Løsningsforslag

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#l%C3%B8sningsforslag)

Bedre tilgang: Lad brugerne selv vælge, hvordan de definerer "bedste" rute under kørsel (runtime).

### Best practices

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#best-practices)

1. Skriv omhyggeligt: Sørg for at krav kun kan fortolkes på én måde
    
2. Selvkritik: Læs dine egne krav igennem og prøv at finde alternative fortolkninger
    
3. Få feedback: Lad andre (især kunder, analytikere og slutbrugere) læse og kommentere kravene
    

Dette afsnit handler om konsistens som en afgørende egenskab ved gode krav:

## Konsistente krav

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#konsistente-krav)

Krav skal være konsistente på to niveauer:

1. Mellem krav: De må ikke modsige hinanden eller skabe uløselige problemer
    
2. Internt i hvert krav: Hvert krav skal være opnåeligt i sig selv
    

#### Eksempel på potentielt inkonsistente krav

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#eksempel-p%C3%A5-potentielt-inkonsistente-krav)

To krav til reparationsaftaler:

- Reducér aftalevinduer til maksimalt 2 timer
    
- Overhold 90% af de planlagte aftaler
    

Problemet: Disse krav kan være umulige at opfylde samtidig (medmindre man ansætter flere reparatører).

#### Kompleksiteten ved store projekter

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#kompleksiteten-ved-store-projekter)

- Parvis konsistens: To krav kan være forenelige hver for sig
    
- Kombinatorisk inkonsistens: Større kombinationer af krav kan være umulige at opfylde samtidig
    
- Ikke altid åbenlyst: I komplekse projekter er inkonsistenser ikke altid umiddelbart synlige
    

#### "Fast, good, cheap - pick two"

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#fast-good-cheap---pick-two)

Klassisk software-udviklings dilemma: Du kan vælge to af følgende tre dimensioner:

1. Hurtigt + høj kvalitet = Dyrt
    
2. Hurtigt + billigt = Lav kvalitet
    
3. Høj kvalitet + billigt = Langsom udvikling
    

#### Best practices

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#best-practices-1)

1. Løbende tjek: Hold nye krav konsistente med eksisterende
    
2. Revider gamle krav: Omskriv ældre krav hvis nødvendigt
    
3. Systematisk gennemgang: Når alle krav er indsamlet, gennemgå dem specifikt for inkonsistenser
    

## Prioritering af krav

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#prioritering-af-krav)

Når projektet skal planlægges, vil du sandsynligvis opdage, at:

- Du ikke har tid eller budget til alle features
    
- Nogle "nice-to-have" funktioner må skæres væk
    
- Du skal træffe svære beslutninger om, hvad der er vigtigst
    

### Prioriteringsproces

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#prioriteringsproces)

Med omkostninger og prioriteter kan du:

- Udskyde dyre, lav-prioritets krav til senere releases
    
- Fokusere på høj-prioritets, lave omkostninger først
    

### Udfordringer med kunder

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#udfordringer-med-kunder)

Kunders typiske reaktion:

- De har svært ved at vælge, hvilke krav de kan undvære
    
- De argumenterer og klager
    
- Det føles som at "vælge hvilket barn der skal fodres til dingo'erne"
    

Virkeligheden: Medmindre budgettet og tidsrammen udvides, skal der træffes beslutninger.

### Prioriterings kategorier

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#prioriterings-kategorier)

Selvom ikke eksplicit nævnt, antyder teksten kategorier som:

- "Must have" - Absolut nødvendige
    
- "Should have" - Vigtige, men ikke kritiske
    
- "Could have" - Ønskelige
    
- "Won't have" - Udelukkes fra denne release
    

### Særlige forhold

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#s%C3%A6rlige-forhold)

Livskritiske systemer (atomreaktorer, flytrafikontrol, rumfartøjer):

- Mange "must have" krav, der ikke kan fjernes
    
- Sikkerhed kan ikke kompromitteres
    
- Juridiske krav, der ikke kan ignoreres
    
- Kosmetiske features kan fjernes (f.eks. automatisk blinklys-annullering), men ikke kritiske systemer (brændstofmonitor, flyveplansberegner)
    

### Realiteten ved softwareudvikling

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#realiteten-ved-softwareudvikling)

Sandsynligheden for implementering:

- "Must" og "Should": Høj sandsynlighed
    
- "Could" og "Won't": Lav sandsynlighed - vil sandsynligvis aldrig blive implementeret
    

Hvorfor: Efter release kommer der bug-rapporter, ændringsanmodninger og nye feature-ønsker, så lavere prioritets-features forbliver på venteliste.

Undtagelse: Store softwarevirksomheder, der pusher nye versioner hvert andet år for at få kunder til at købe noget - disse når nogle gange ned i "could" og "won't" kategorierne, og måske endda "why?" og "you must be joking!" kategorierne.

Dette afsnit forklarer MoSCoW-metoden - et populært system til prioritering af krav:

## MoSCoW

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#moscow)

### M - Must (Skal have)

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#m---must-skal-have)

Defintion: Påkrævede funktioner, der skal inkluderes

- Nødvendige for at projektet kan betragtes som en succes
    
- Kan ikke udelades uden at kompromittere projektets grundlæggende formål
    
- Eksempel: Login-funktionalitet i et sikkerhedssystem
    

### S - Should (Bør have)

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#s---should-b%C3%B8r-have)

Definition: Vigtige funktioner, der bør inkluderes, hvis muligt

- Kan udskydes til release 2, hvis der er en workaround
    
- Ikke helt kritiske, men stadig meget vigtige
    
- Eksempel: Avanceret søgefunktionalitet i et bibliotekssystem
    

### C - Could (Kunne have)

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#c---could-kunne-have)

Definition: Ønskelige funktioner, der kan udelades

- Kan skubbes til release 2, men har lavere prioritet end "Should"-funktioner
    
- Risiko for aldrig at blive implementeret
    
- Eksempel: Temaer og farvetilpasning i en applikation
    

### W - Won't (Vil ikke have)

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#w---wont-vil-ikke-have)

Definition: Helt valgfrie funktioner, som kunden har accepteret ikke inkluderes i nuværende release

- Kan måske inkluderes i fremtidige releases, hvis tid tillader det
    
- Nogle gange kun med for at "gøre en særligt højlydt og politisk forbundet kunde glad"
    
- Realitet: Ofte ingen intention om nogensinde at implementere disse
    

### Praktiske fordele

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#praktiske-fordele)

1. Klar kommunikation med kunder om prioriteter
    
2. Lettere beslutningstagning når tid/budget bliver stramt
    
3. Struktureret tilgang til scope-management
    
4. Diplomatisk håndtering af mindre vigtige kundeønsker
    

## Verificerbare krav

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#verificerbare-krav)

Krav skal være verificerbare - du skal kunne bevise om de er opfyldt eller ej. Hvis du ikke kan verificere et krav, hvordan ved du så, om du har opfyldt det? Og vigtigere: hvordan beviser du det over for kunden?

### Egenskaber ved verificerbare krav

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#egenskaber-ved-verificerbare-krav)

Skal være:

- Afgrænsede og præcist definerede
    
- Målbare med konkrete kriterier
    
- Ikke åbne eller vage udsagn
    

### Eksempel: Fra vagt til verificerbar

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#eksempel-fra-vagt-til-verificerbar)

Dårligt (ikke-verificerbart) krav: "Behandl flere arbejdsordrer per time, end der behandles i øjeblikket"

Problemerne:

- Hvad betyder "flere"?
    
- 1 mere per time? 100 mere? 1.000 mere?
    
- Teknisk set opfylder "én mere" kravet, men tilfredsstiller sandsynligvis ikke kunden
    

Bedre (verificerbart) krav: "Behandl mindst 100 arbejdsordrer per time"

Fordelen: Relativt let at afgøre om programmet opfylder dette krav.

## REQUIREMENT CATEGORIES

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#requirement-categories)

1. Audience-Oriented Requirements  
    Målgruppeorienterede krav - krav der er skrevet til specifikke interessenter
2. Business Requirements  
    Forretningskrav - overordnede forretningsmål og -behov
3. User Requirements  
    Brugerkrav - hvad slutbrugerne har brug for fra systemet
4. Functional Requirements  
    Funktionelle krav - konkrete funktioner og features systemet skal have
5. Nonfunctional Requirements  
    Ikke-funktionelle krav - kvalitetsegenskaber som performance, sikkerhed, brugervenlighed
6. Implementation Requirements  
    Implementeringskrav - tekniske begrænsninger og implementerings detaljer

## Common requirements

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#common-requirements)

### Brugergrænseflader og Navigation

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#brugergr%C3%A6nseflader-og-navigation)

Skærme/Forms:  
Hvilke skærme eller formularer er nødvendige?

Menuer:  
Hvilke menuer skal skærmene have?

Navigation:

- Hvordan navigerer brugerne gennem systemet?
    
- Knapper, menuer, frem/tilbage-pile eller en kombination?
    

### Dataflow og Arbejdsprocesser

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#dataflow-og-arbejdsprocesser)

Workflow:

- Hvordan bevæger data sig gennem systemet?
    
- Arbejdsordrer, indkøbsanmodninger, fakturaer osv.
    

### Sikkerhed og Adgangskontrol

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#sikkerhed-og-adgangskontrol)

Login:

- Hvordan gemmes og valideres login-oplysninger?
    
- Adgangskodeformater (kræver bogstav, tal, specialtegn, emoji)
    
- Regler (skal skiftes månedligt)
    

Brugertyper:

- Forskellige brugerroller (indtastningsmedarbejder, forsendelsesmedarbejder, supervisor, admin)
    
- Forskellige rettigheder for hver rolle?
    

### Sporing og Overvågning

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#sporing-og-overv%C3%A5gning)

Audit tracking og historik:

- Skal systemet spore, hvem der lavede ændringer?
    
- Eksempel: Se hvem der ændrede en kunde til premium-status eller gav 99% rabat
    

### Dataforvaltning

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#dataforvaltning)

Arkivering:

- Skal systemet arkivere ældre data for at frigøre plads i den aktive database?
    
- Kopiering til data warehouse til analyse?
    

### Systemadministration

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#systemadministration)

Konfiguration:

- Skal applikationen have konfigurations-skærme for systemadministratorer?
    
- Redigering af produktdata, forsendelse/håndteringspriser, algoritme-parametre
    
- Vigtig note: Hvis du ikke bygger disse skærme, skal du lave ændringerne for kunderne senere
    

### De fem W'er (og ét H):

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#de-fem-wer-og-%C3%A9t-h)

- Who (Hvem) (Brugere eller kunder)
    
- What (Hvad) (er behovet)
    
- When (Hvornår)(skal det leveres)
    
- Where (Hvor)(skal applikationen bruges. eks. på et kontor, på en tablet ….)
    
- Why (Hvorfor)(afdække og verificere behovene)
    
- How (Hvordan)(kig ud af boksen for alternative løsninger,evt. kunden har ideer. Nogle gange er den mest elegante løsning at forbedre det, der allerede fungerer, i stedet for at genopfinde det hele.)
    

## Refining requirements

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#refining-requirements)

### Efter indledende research

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#efter-indledende-research)

Når du har:

- Talt med kunder og brugere
    
- Observeret brugerne på arbejde
    
- Stillet irriterende spørgsmål, indtil de er trætte af dig
    

Resultat: Du skulle have god forståelse af brugernes nuværende drift og behov Hvis ikke: Stil flere spørgsmål og observér mere

### Fra mål til tilgange

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#fra-m%C3%A5l-til-tilgange)

Næste skridt: Brug din viden til at udvikle idéer til løsning af brugerens problemer

Transformation:

- Mål (hvad kunderne skal gøre)
    
- Tilgange (hvordan applikationen skal gøre det)
    

### Fra højt til lavt niveau

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#fra-h%C3%B8jt-til-lavt-niveau)

Højt niveau (acceptabelt i starten):

- Krav som "Behandl kunderegistre"
    
- Fleksibelt og åbent for mange løsninger
    

Lavt niveau (nødvendigt til slut): Konkrete beslutninger om:

- Hvordan brugere vælger poster til redigering
    
- Hvilke skærme de bruger
    
- Hvordan de navigerer mellem skærmene
    

## Konkrete krav

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#konkrete-krav)

Disse beslutninger fører til krav, der beskriver:

- Formularer
    
- Navigationsteknikker
    
- Andre funktioner applikationen skal levere
    
- Alt hvad brugerne har brug for for at udføre deres job
    

## Validering og verifikation

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#validering-og-verifikation)

### Definitioner

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#definitioner)

Krav Validering (Requirements Validation): Processen med at sikre, at kravene siger de rigtige ting

Krav Verifikation (Requirements Verification): (Definition ikke inkluderet i dette uddrag, men typisk: at sikre det byggede system opfylder kravene)

### Krav Validering 

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#krav-validering)

Hvem udfører det:

- Kunder
    
- Analytikere
    
- Brugere
    

Hvad der kontrolleres: Alle krav gennemgås for at sikre, at de:

1. Beskriver ting applikationen skal gøre (korrekte krav)
    
2. Beskriver alt applikationen skal gøre (komplette krav)
    

### To kritiske dimensioner

[](https://github.com/bofl2002/Obsidian/blob/main/Krav.md#to-kritiske-dimensioner)

Korrekthed:

- Er de beskrevne funktioner relevante?
    
- Skal applikationen virkelig gøre disse ting?
    

Komplethed:

- Mangler der vigtige funktioner?
    
- Er alle nødvendige aspekter dækket?