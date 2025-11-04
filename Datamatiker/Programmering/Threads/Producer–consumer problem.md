# Producer-Consumer Problemet i C# med Threads

Producer-Consumer-problemet er et klassisk synkroniseringsproblem i multithreading-programmering. Det opstår, når en eller flere producere genererer data, og en eller flere forbrugere henter og behandler disse data. Udfordringen er at sikre, at producere ikke overskriver en delt ressource, mens forbrugere ikke forsøger at hente data, der endnu ikke er tilgængelige.
 
### Problemer og Synkronisering

Hvis producere genererer data hurtigere end forbrugere kan behandle dem, kan en buffer blive overfyldt.  
Hvis forbrugere arbejder hurtigere end producere, kan de forsøge at læse fra en tom buffer.  
Der skal bruges synkroniseringsmekanismer for at undgå race conditions og sikre, at producer og consumer arbejder korrekt sammen.
 
# Løsning i C# med BlockingCollection

C# har en indbygget klasse, BlockingCollection\<T\>, der gør det nemt at implementere Producer-Consumer mønstret. Den fungerer som en trådsikker kø.
 
**Eksempel med BlockingCollection:**

![Exported image](Exported%20image%2020251104230625-0.png)  

### Hvordan virker koden?

- BlockingCollection\<int\> bruges som buffer med en kapacitet på 5.
- Producenten (producerTask) genererer tal og tilføjer dem til queue, men stopper hvis køen er fuld.
- Forbrugeren (consumerTask) læser fra køen og arbejder langsommere end producenten.
- queue.CompleteAdding() fortæller forbrugeren, at der ikke kommer flere elementer.
- GetConsumingEnumerable() sikrer, at forbrugeren venter, indtil der er data tilgængelig.
 
# Løsning med Monitor og Queue\<T\>

Hvis du vil have mere kontrol, kan du bruge Monitor til manuel synkronisering.
 
**Eksempel med Monitor:**

![Exported image](Exported%20image%2020251104230629-1.png) ![Exported image](Exported%20image%2020251104230630-2.png)  

### Hvordan virker denne løsning?

Producenten låser objektet (locker), tilføjer et element til køen og vækker en ventende tråd med Monitor.Pulse().  
Forbrugeren låser objektet, og hvis køen er tom, venter den med Monitor.Wait(), indtil en producent tilføjer noget.  
Monitor.Wait() gør, at forbrugeren ikke bruger CPU, mens den venter.
 
# Hvornår skal du bruge hvad?

- Brug BlockingCollection\<T\> → Hvis du vil have en enkel, trådsikker løsning.
- Brug Monitor + Queue\<T\> → Hvis du vil have mere kontrol over synkroniseringen.
- Brug SemaphoreSlim eller AutoResetEvent → Hvis du har brug for avanceret signalering mellem tråde.