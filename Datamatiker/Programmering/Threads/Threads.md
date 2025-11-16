[BroCode](https://www.youtube.com/watch?v=rUbmW4qAh8w&t=13s)  
[Threading In C#](https://drive.google.com/file/d/12MG6ABY1gIxal9JQgvTWUctMDOq6zWGn/view?usp=drive_link)  
[Singleton Pattern](https://csharpindepth.com/articles/singleton)
   

# Introduktion til Threads i C#
 
I C# bruges threads til at køre flere operationer parallelt, hvilket muliggør bedre udnyttelse af CPU'en og kan forbedre programmets ydeevne, særligt i scenarier hvor der er mange samtidige opgaver.
 
## Hvad er en Thread?
 
En thread er en eksekveringsenhed inden for en proces. En C#-applikation starter som standard med en enkelt thread, kaldet "main thread". For at eksekvere flere dele af programmet samtidig kan flere threads oprettes og køres samtidigt.
 
## Oprettelse af en Thread
 
Threads i C# kan oprettes ved hjælp af System.Threading.Thread-klassen:
 ![using using class System System Threading Program ...](Exported%20image%2020251104230613-0.png)   
I ovenstående eksempel oprettes en ny thread, der eksekverer DoWork-metoden.
 
## Arbejde med Threads
 
Når der arbejdes med flere threads, skal man være opmærksom på:
 
**Synkronisering**:  
Flere threads, der deler ressourcer, kan føre til race conditions, hvor resultatet afhænger af, hvilken tråd der eksekverer først. Dette kan føre til uventet adfærd og fejl i programmet. For at undgå race conditions kan man benytte synkroniseringsmekanismer såsom lock, Monitor eller Mutex.
 
## Lock:

lock er en simpel mekanisme til at sikre, at kun én thread ad gangen kan få adgang til en sektion af koden:
 ![Exported image](Exported%20image%2020251104230617-1.png)  

Lock(this) skal bruges varsomt da man låser på selve instansen af et objekt. Derved kan man låse objekter som bliver brugt andre steder i ens kode uvidende og lave 'Deadlock' eller 'Race conditions'.  
I stedet bør man som ovenover oprette et private readonly objekt som ingen andre end klassen selv kan tilgå og derved har man større kontrol over hvem og hvor det bliver brugt.   En anden måde at sikre ens værdier er at bruge **Interlocked.Increment** som vist nedenunder:

![Exported image](Exported%20image%2020251104230618-2.png)

Perfomance mæssigt er **Interlocked** bedre da **lock** låser objekter i hele løsningen og kan gøre programmet langsommere.
 
**🔹** **Fordele: Nem at bruge og effektiv til mindre kritiske sektioner.**  
**🔹** **Ulemper: Kan føre til deadlocks, hvis tråde venter på flere låse samtidig.**
 
## Monitor klassen:

Monitor giver mere kontrol end lock, da det tillader timeout og manuel frigivelse af låsen:
 ![class Safecounter private int count c private read...](Exported%20image%2020251104230620-3.png)  

**🔹** **Fordele: Fleksibel og understøtter mere avancerede scenarier.**  
**🔹** **Ulemper: Mere kompleks end lock.**
 
## Mutex:
 
Mutex bruges, når tråde fra forskellige processer skal synkroniseres. Den er langsommere end lock, men kan bruges på tværs af processer:
 ![class Program private static static void Main for ...](Exported%20image%2020251104230621-4.png)  

**🔹** **Fordele: Kan synkronisere mellem processer.**  
**🔹** **Ulemper: Langsommere end lock og Monitor.**
   

**Thread Safety:**  
For at sikre, at kun én thread ad gangen har adgang til en delt ressource, kan man anvende lock-statementet eller Monitor-klassen:
 ![class Safecounter private int _count 0 private rea...](Exported%20image%2020251104230622-5.png)  

**Background Threads:**  
En thread kan markeres som en baggrundstråd ved at sætte dens IsBackground-egenskab til true. Baggrundstråde afsluttes automatisk, når hovedtråden afsluttes, hvilket betyder, at de ikke forhindrer programmet i at lukke:
 ![Thread backgroundThread new ThreadDoWork backgroun...](Exported%20image%2020251104230623-6.png)  

Dette er nyttigt for tråde, der udfører sekundære opgaver, såsom logning eller periodisk vedligeholdelse, uden at blokere programmets afslutning.
 
## Alternativer til Threads
 
C# tilbyder flere moderne alternativer til at arbejde med tråde, bl.a. Task-klassen fra System.Threading.Tasks og async/await-mønsteret, som gør det lettere at arbejde med asynkrone operationer uden at håndtere threads direkte.
 
Konklusion
 
Threads er en kraftfuld mekanisme i C#, men det kræver omhu at arbejde med dem for at undgå problemer som race conditions og dødlås. I mange tilfælde er Task og async/await bedre alternativer, da de er lettere at bruge og reducerer kompleksiteten i parallel programmering.