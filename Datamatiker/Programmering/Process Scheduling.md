Process Scheduling er en vigtig del af et operativsystems CPU scheduling-mekanisme, der håndterer eksekvering af processer og trådmanagement. Det sikrer optimal udnyttelse af CPU'en ved at organisere, hvilken proces der skal køre næste.
 
# Typer af Process Scheduling:
 
### Long-term scheduling:
 
Bestemmer, hvilke processer der skal indlæses i ready queue fra job queue.  
Styrer graden af multiprogramming (hvor mange processer, der er i RAM samtidig).  
Bruges primært i batch-systemer.
 
### Short-term scheduling (CPU scheduling):
 
Vælger, hvilken proces fra ready queue, der får CPU'en næste gang.  
Udføres ofte og skal være hurtig.  
Anvender forskellige scheduling algorithms (fx Round Robin, Shortest Job Next, Priority Scheduling).
 
### Medium-term scheduling:
 
Midlertidigt fjerner processer fra RAM (suspension) for at optimere ressourceforbrug.  
Bruges i systemer med swapping (processer flyttes til/fra disk for at frigøre hukommelse).  
Scheduling Criteria  
For at evaluere en CPU scheduler, anvendes forskellige målekriterier:
 
CPU utilization – Hvor effektivt CPU’en bruges.  
Throughput – Antal færdiggjorte processer pr. tidsenhed.  
Turnaround time – Tid fra job submission til færdiggørelse.  
Waiting time – Tid en proces bruger i ready queue.  
Response time – Tid fra en proces indsendes til første respons gives.
 
# Scheduling Algorithms

### First-Come, First-Served (FCFS):

Processer køres i den rækkefølge, de ankommer.

### Shortest Job Next (SJN):

Processen med kortest burst time eksekveres først.

### Round Robin (RR):

Processer får CPU'en i en fast time quantum, derefter skiftes til næste.

### Priority Scheduling:

Processer prioriteres baseret på en tildelt værdi.

### Multilevel Queue Scheduling:

Processer inddeles i flere køer med forskellige algoritmer.  
Process Scheduling er afgørende for et systems ydeevne og responsivitet, især i time-sharing systems.
 
# Time Sharing:
 
Time-sharing i CPU er en teknik, der gør det muligt for flere processer at dele CPU’en ved at skifte hurtigt mellem dem. Det anvendes i multitasking-operativsystemer, hvor flere brugere eller applikationer kan køre samtidig uden mærkbar forsinkelse.
 
### Hvordan fungerer Time-Sharing?

### Preemptive Scheduling:
 
CPU'en skifter mellem processer baseret på en time quantum (en fast tidsperiode).  
Når time quantum udløber, afbrydes processen, og CPU’en tildeles en anden proces.  
Dette sikrer, at ingen proces monopoliserer CPU’en.
 
### Context Switching:
 
Når CPU’en skifter fra én proces til en anden, gemmes den nuværende proces' tilstand (register values, program counter, stack).  
Den nye proces' tidligere tilstand indlæses, så den kan fortsætte fra, hvor den slap.
 
### Round Robin Scheduling (ofte brugt i time-sharing):
 
Hver proces får en fast time slice (fx 10 ms).  
Efter tidsrummet skiftes til næste proces i ready queue.  
Processer, der ikke er færdige, sættes tilbage i køen.