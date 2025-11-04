# Indhold i en PCB

En PCB indeholder typisk følgende information:
 
### 1. Process Identification (PID)

Hver proces får tildelt et unikt Process ID (PID), som bruges til at identificere den.  
Kan også inkludere en Parent Process ID (PPID), hvis processen er oprettet af en anden proces.

### 2. Process State

Angiver den nuværende tilstand af processen, som kan være:  
**New** – Processen er oprettet, men endnu ikke klar til eksekvering.  
**Ready** – Klar til at køre, men venter på CPU.  
**Running** – Kører på CPU’en.  
**Waiting** (Blocked) – Venter på en I/O-operation eller en anden begivenhed.  
**Terminated** – Processen er afsluttet.

### 3. CPU Registers

Indeholder værdierne af CPU’ens registers (fx Program Counter, Stack Pointer, General-Purpose Registers) på det tidspunkt, hvor processen blev afbrudt.  
Bruges under context switching for at gendanne processens tilstand.

### 4. CPU Scheduling Information

Data, der hjælper CPU scheduler med at bestemme, hvilken proces der skal eksekveres næste.  
Kan indeholde:  
Process priority (bruges i priority scheduling).  
Scheduling queue pointers (for at placere processen i en ready queue).

### 5. Memory Management Information

Indeholder oplysninger om processens hukommelsesforbrug, såsom:  
Base and limit registers (angiver processens hukommelsesområde).  
Page tables (bruges i systemer med virtual memory).

### 6. I/O Status Information

Sporer hvilke I/O devices og files processen bruger.  
Kan inkludere oplysninger om open files, allocated devices og pending I/O requests.
 
# PCB’s Rolle i Process Management
 
### 1. Context Switching

Når CPU’en skifter fra én proces til en anden, gemmes den nuværende process’ tilstand i dens PCB.  
Den nye process’ PCB indlæses, så CPU’en kan fortsætte eksekveringen fra, hvor den slap.

### 2. Process Creation and Termination

Når en proces oprettes, tildeler operativsystemet en ny PCB.  
Når processen afsluttes, fjernes PCB’en fra systemet.

### 3. Scheduling and Resource Allocation

CPU scheduler bruger PCB til at afgøre, hvilken proces der skal køre næste.  
Hukommelses- og I/O-styring anvender PCB til at holde styr på, hvilke ressourcer en proces bruger.