#### 1. Kort intro af AirPlate som virksomhed
AirPlate er en dansk ejet start up som arbejder i feltet omkring sporing og logging af drone aktivitet. 
De har udviklet deres egne scannere/sensorer til at spore aktiviteten og har udviklet deres egen web applikation og native app til visning og logging.
#### Tech baggrund:
Når en drone letter udsender den et RemoteID signal, som er en industri standard, som bliver sendt over WiFi/Bluetooth. Dette indeholder en MAC adresse og et sæt felter: 
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

Fra første dag har der været stor frihed under ansvar. Jeg fik ret hurtigt den første opgave, et lille sammenlignings label på forsiden. Kort fortalt bliver der sammenlignet i forhold til den tilsvarende forudgående periode, altså her bliver de sidste 3 dage sammenlignet med de 3 dage før det.
![[Pasted image 20260923084059.png]]
![[Indsat billede.png]]



#### 3. Hoved feature - Zone backfill og prioritering
- **Database setup - Influx/Timeseries og MySQL:**
  Influx holder alle punkter som bliver logget og MySQL holder kun et repræsentativt punkt for hver flyvning. 
  Rå punkter streamer ind i influx, herefter kører der to jobs, time mæssigt og døgn baseret, som laver dem til komplette flyvninger med zoner og locations data og skriver dem til MySql og laver en række per flyvning. 
- Begge databaser har data som den anden ikke har. Influx har f.eks hele rækken af punkter som sensoren har logget fra dronen, men basalt set er det ikke logget som en flyvning, kun en bucket af punkter/measurements med nogle drone informationer.
- MySQL derimod, samler al informationen sammenlignet på enten MAC/Takeoff punkt(OcuSync droner), start og stop koordinat, men opbevarer kun et repræsentativt punkt for flyvningen. Det er disse 3 felter der fungerer som nøgler imellem de to databaser.
- Derfor er det også nødvendigt for at kunne få det fulde billede at kigge i begge databaser. 
#### 4. Refleksion over praktikken
Overordnet set har det været en givende oplevelse at være i praktik. Endnu engang har jeg opdaget noget nyt omkring mig selv fag mæssigt. 
Hele aspektet med at den "daglige vedligeholdelse" af software er faktisk ikke nødvendigvis den vej jeg skal gå. 
Jeg synes faktisk det er enormt spændende at designe og opstarte systemer, som vi har gjort en del gange i løbet af uddannelsen. 
For lige at klemme AI bølgen ind i dette dokument, er det også min overbevisning at det er et af vigtigste punkter i dag når det handler om software udvikling. 
"*Garbage in, garbage out*"
Med et veldesignet grundsystem at læne sig op af, vil videreudviklingen og vedligeholdelsen være umådeligt meget nemmere.