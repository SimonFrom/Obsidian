RAM (Random Access Memory) spiller en central rolle i forhold til CPU’en og processer i et computersystem. Her er en forklaring af, hvordan de arbejder sammen:
   

### Execution Context:

Når en CPU eksekverer en proces, henter den instruktioner og data fra RAM. RAM fungerer som et hurtigt mellemlager mellem den langsommere permanent storage (HDD/SSD) og CPU’en.
 
### Process Address Space:

Hver proces får tildelt et virtuelt adresserum i RAM af operativsystemet. Dette inkluderer kode, heap (dynamisk allokeret hukommelse), stack (funktionkald og lokale variabler) og data segmenter.
 
### Memory Management Unit (MMU):

CPU’en bruger en MMU til at oversætte virtuelle adresser til fysiske adresser i RAM. Hvis data ikke findes i RAM, opstår en page fault, hvilket kan føre til swapping (data flyttes mellem RAM og disk).
 
### Cache and RAM Interaction:

CPU’en har interne caches (L1, L2, L3) for at reducere latens ved at gemme ofte brugte data fra RAM. Hvis en CPU ikke finder de nødvendige data i cache, henter den dem fra RAM, hvilket tager længere tid.
 
### Multitasking & Context Switching:

Når flere processer kører, skifter CPU’en mellem dem gennem context switching. Hver proces’ tilstand (registerværdier, program counter osv.) gemmes midlertidigt i RAM, så den kan genoptages senere.
 
### Memory Bottlenecks:

Hvis RAM-mængden er for lav, tvinges systemet til at bruge swap-space (page file), hvilket reducerer ydelsen drastisk, da disk I/O er meget langsommere end RAM.
 
Kort sagt er RAM en afgørende komponent, der gør det muligt for CPU’en at tilgå og eksekvere processer hurtigt uden konstante diskadgange.