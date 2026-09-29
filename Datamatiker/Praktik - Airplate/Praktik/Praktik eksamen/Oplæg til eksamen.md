#### 1. Kort intro af AirPlate som virksomhed
AirPlate er en dansk ejet start up som arbejder i feltet omkring sporing og logging af drone aktivitet. 
De har udviklet deres egne scannere/sensorer til at spore aktiviteten og har udviklet deres egen web applikation og native app til visning og logging.
#### Tech baggrund:
Når en drone letter og er igang med en flyvning udsender den et RemoteID signal, som er en industri standard, som bliver sendt over WiFi/Bluetooth. Dette indeholder en MAC adresse og et sæt felter: 
- Længde og bredde grader
- Højde
- Hastighed
- Operatør ID
MAC adressen bruges som det primære ID igennem programmet efterfølgende.
Firmaet DJI har lavet deres egen standard, OcuSync. Her bliver der ikke sendt MAC, istedet sender den et takeoff punkt, dette punkt fungerer som ID igennem programmet istedet for.

#### 2. Hvad har jeg lavet
Stillingen har været fullstack udvikler, så jeg har været i berøring med næsten alle aspekter indenfor software udvikling, fra ide til udførelse. 
Flowet kan beskrives i 4 trin:
1. **Idé**
   Her blev den grove skitse lavet som vi kender det fra pre sprint i SCRUM f.eks. 
   Det kunne også være at der allerede var et velbeskrevet ønske, som åbnede op for opfølgende spørgsmål. Acceptance Criteria bliver opstillet.
2. **Design - Iterativt** 
   Skitsen bliver præciseret og bevæger sig over i black box. Research omkring ny teknologi bliver lavet. Ændringer til databasen vil også blive produceret her i form af migration scripts.
3. **Implementering - Iterativt**
   Koden bliver lavet og testet. Nogle tilfælde kræver en iterativ tilgang mellem **Design** og **Implementering**, for at løse udfordringer der opstår. 
4. **Aflevering/Feedback - Iterativt**
   Arbejdet sendes til review af hele teamet. Igen kan en iterativ tilgang være nødvendig for at tilfredsstille både interne stakeholders, men også kundens behov. 

Af konkrete opgaver jeg har haft og arbejdet med kan nævnes:

1. **Labels til sammenligning af data perioder** 
Min første opgave var tilføje labels som nemt kunne visning en stigning eller et fald i drone aktivitet, direkte på forsiden. Ideen var det skulle holdes simpelt give et overblik uden at man skulle finde og sammenligne tal. En fin frontend opgave at starte ud på, da alt data var tilgængelig fra backend allerede.
![[Indsat billede 1.png]]
**Refleksion:** 
I og med dette var min første kontakt med kode basen var den store udfordring at finde rundt og identificere de korrekte lister og variabler jeg kunne bruge. Min grundviden omkring software arkitektur og komponent opbygning hjalp mig godt på vej her.

2. **Opdatering af pdf rapport generering**
Næste opgave bød på at jeg skulle lave mulighed for at organisationer som kun har sensorer og ikke abonnerer på alarm zoner kunne generere en rapport med aktivitet.
**Refleksion:**
Dette var også ren frontend, men bød på en masse brug af If-Else udtryk at skelne imellem den ene og den anden type.

3. **Gruppering af sensorer i brugerskabte grupper**
Sensorene havde ikke nogen inddeling før, kun delt op på organisations niveau.
Jeg designede en ny tabel til MySQL databasen, med tanke på normalisering og sikring af korrekthed i databasen ved hjælp af cascading deletes. Denne opgave gav også en lektie i vigtigheden af kravs afklaring/acceptance criteria. Det GitHub issue jeg arbejdede ud fra viste sig ikke at stemme helt overens med det ønskede udtryk og det endte med en god refaktorering inden endelig godkendelse. 
**Refleksion:**
Jeg fik brugt min viden omkring database design, herunder primary, foreign og composite keys og normaliserings former(Læs op og tjek) og så endnu engang vigtigheden af at et team ensretter i form af projektstyring.

4. **Filtrerings funktion til visningen af live droner**
På forsiden viser der live hvilke droner der spores i øjeblikket. Der kan være 1 eller 100. For at give brugerne mulighed for kun at se relevante informationer, var der et ønske om at kunne vælge imellem hvor meget og hvilken aktivitet der skulle vises eller ikke vises. 
![[Pasted image 20260929082850.png]]
Dette krævede en undersøgelse af hvordan websocket forbindelsen virkede og hvad der blev sendt. Det viste sig at websocket forbindelsen alene ikke var nok. Der er ikke nogle oplysninger omkring hvilke alarmzoner dronen har krydset og det krævede kontakt til både Influx timeseries databasen og MySQL databasen. Derfra valgte jeg at tilføje alarm_zone_id til websocket forbindelsen og bruge den variabel til at tjekke.
**Refleksion:**
Jeg fik brugt min erfaring for at analysere og researche/undersøge hvad en løsning på et problem kan være for derfter at fremstille løsnings forslag til samarbejdspartnere/teammedlemmer.

5. **Backfill af aktivitet i ny oprettede zoner**
Når man opretter en ny alarm zone vil detektionerne altid kun være fremadrettet. Man skal ikke tænke langt for at forestille sig at et firma der ejere nogle sensorere, kunne tænke sig at vide hvad der er foregået før oprettelsen. Med de mobile sensorer vil det også kunne være med til at give et billede af aktiviteten i området man befinder sig i. Der er også tænkt afgrænsning/private oplysninger ind i hentningen, en organisation har kun adgang til data fra sensorer som de i forvejen har adgang til. Det er ikke alle kunder der er interesserede i at andre kan se aktiviteten i deres område.  
**Refleksion:**
Under udvikling er det vigtigt at have i tankerne at, bare fordi at det er data du har i systemet eller databasen, er det ikke nødvendigvis "din data" som du har fuld kontrol eller råderet over. I AirPlates tilfælde er der f.eks nogle områder som ikke skal være offentligt kendte, så derfor er det vigtigt med afgrænsning.

#### 3. Refleksion over praktikken
Overordnet set har det været en givende oplevelse at være i praktik. Endnu engang har jeg opdaget noget nyt omkring mig selv fag mæssigt. 
Hele aspektet med at den "daglige vedligeholdelse" af software er faktisk ikke nødvendigvis den vej jeg skal gå. 
Jeg synes faktisk det er enormt spændende at designe og opstarte systemer, som vi har gjort en del gange i løbet af uddannelsen. 
For lige at klemme AI bølgen ind i dette dokument, er det også min overbevisning at det er et af vigtigste punkter i dag når det handler om software udvikling. 
"*Garbage in, garbage out*"
Med et veldesignet grundsystem at læne sig op af, vil videreudviklingen og vedligeholdelsen være umådeligt meget nemmere.

En af de største udfordringer var helt klart i starten, da jeg skulle sætte mig ind i en helt ny forudviklet kodebase. For alle, uanset erfarings niveau vil jeg tro, vil det være en stor opgave og med min mængde erfaring, er det så bare en endnu større opgave. Det føltes faktisk som et enormt stort pres og der var da dage hvor jeg tænkte om "jeg overhovedet havde lært nok" på uddannelsen og var klar til både praktik og reelt arbejde lige om lidt. 
Her små 3 måneder efter føler jeg stadig ikke at jeg har helt styr på det, jeg har selvfølgelig en meget bedre forståelse end da jeg startede, men jeg føler slet ikke at jeg kan sige at jeg er i mål med den del. 

Det har også været spændende at komme ud og se en anden tilgang til software udvikling på team delen. 
Fra uddannelsen kender vi jo til forskellige strukturer og projektstyrings værktøjer og har generelt været vant til en langsommere fremgang, både på grund af erfaringsgrundlaget men også brugen af AI. I det relativt lille team her hos AirPlate fungerer det fint, men det har også været tydeligt for mig at se at der skal ikke være mange flere udviklere ind over før at en streng disciplin, koordination og planlægning mellem features og ændringer er en fundamental nødvendighed når der i kraftig grad anvendes AI.

