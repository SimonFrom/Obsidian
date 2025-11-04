Indtil nu har vi set på, hvordan man starter og konfigurerer en thread, samt hvordan data kan sendes i begge retninger. Vi har også gennemgået, hvordan lokale variabler er private for en thread, og hvordan referencer kan deles mellem threads for at muliggøre kommunikation. Næste skridt er synchronization, hvilket er nødvendigt for at koordinere threads' handlinger og sikre et forudsigeligt resultat.
 
# Fire kategorier af synkronisering

### Simple blocking methods:

En thread venter på en anden thread eller en tidsperiode.  
Eksempler: Thread.Sleep, Thread.Join, Task.Wait.
 
### Locking constructs:

Begrænser, hvor mange threads der kan tilgå en given sektion af kode.  
Exclusive locks: Kun én thread kan få adgang ad gangen (f.eks. lock, Monitor.Enter/Exit, Mutex).
 
### Nonexclusive locks:

Flere threads kan få adgang under visse betingelser (f.eks. Semaphore, ReaderWriterLock).
 
### Signaling constructs:

En thread venter på et signal fra en anden, i stedet for at bruge ineffektiv polling.  
Eksempler: EventWaitHandle, Monitor.Wait/Pulse, CountdownEvent, Barrier (fra .NET 4.0).
 
### Nonblocking synchronization constructs

Bruger CPU-instruktioner til at synkronisere adgang til delt data.  
Eksempler: Thread.MemoryBarrier, volatile keywordet, Interlocked-klassen.
 
### Blocking:

En thread er blocked, når den venter på en betingelse (f.eks. Thread.Sleep eller Thread.Join). Mens en thread er blokeret, bruger den ingen CPU-tid.
 
### En thread kan unblockes på fire måder:
 
1. Når betingelsen opfyldes.
2. Når en timeout udløber.
3. Via Thread.Interrupt.
4. Via Thread.Abort.
 
### Blocking vs. Spinning:

I stedet for at blokere kan en thread "spinne" i en løkke og vente:
 
```
while (!proceed);
```
 
Dette er dog ineffektivt, da CPU’en tror, at tråden udfører noget vigtigt. En hybridløsning er at bruge Thread.Sleep(10), men det kan føre til problemer med concurrency.
 
### ThreadState:

ThreadState egenskaben kan bruges til at tjekke en threads status:

![Exported image](Exported%20image%2020251104230634-0.png)  

Dette er dog kun nyttigt til diagnostik, da en threads tilstand kan ændre sig hurtigt.
 _Locking – Eksklusiv låsning_

Eksklusive låse sikrer, at kun én thread ad gangen kan køre en bestemt sektion af kode. Det gøres typisk med lock eller Mutex:
 
**Ikke-thread-safe kode:**

![class Threadunsafe static static if int vall void ...](Exported%20image%2020251104230635-1.png)  

Her kan en anden thread potentielt sætte _val2 til 0 mellem if-tjekket og Console.WriteLine, hvilket kan føre til division by zero.
 
**Thread-safe version med lock:**

![class Threadsafe static static static readonly obj...](Exported%20image%2020251104230636-2.png)  

Kun en thread kan bruge _lock objektet ad gangen og garantere derved at val ikke kan blive sat andre steder samtidigt.