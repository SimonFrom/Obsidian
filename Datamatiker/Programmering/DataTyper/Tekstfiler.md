[Read and write to files](https://learn.microsoft.com/en-us/troubleshoot/developer/visualstudio/csharp/language-compilers/read-write-text-file)  
[while Iterations and Reading Data from a Text File](https://learn.microsoft.com/da-dk/shows/c-fundamentals-for-absolute-beginners/12)  
[String Split](https://www.dotnetperls.com/split)  
[File.ReadAllLines method](https://learn.microsoft.com/en-us/dotnet/api/system.io.file.readalllines?view=netcore-3.1#code-try-1)  
[File Class](https://learn.microsoft.com/en-us/dotnet/api/system.io.file?view=netcore-3.1)  
[File.Open and File.Close](https://www.linkedin.com/learning/object-oriented-programming-with-c-sharp/file-open-and-close?u=57075649)  
[File.Close in finally block](https://www.linkedin.com/learning/object-oriented-programming-with-c-sharp/file-close-in-the-finally-block?u=57075649)  
[Keyword Using](https://www.linkedin.com/learning/object-oriented-programming-with-c-sharp/keyword-using?u=57075649)  
[Using objects that implement IDisposable](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/using-objects)  
[FilePaths](https://learn.microsoft.com/en-us/dotnet/standard/io/file-path-formats)  
[FilePaths, Relative vs Absolut](https://www.youtube.com/watch?v=qh5LhpaKC-c&t=72s&ab_channel=KampaPlays)
 
Side 73 - 78 i Yellow Book
 
- Tilføj tekstfiler direkte i Visual Studio i add item så den altid følger projektet.
- Vær sikker på at filen findes i bin/debug folder. Tjek i properties og vælg "copy if newer" i "Copy to output directory".
 
Basic text reader:  
Husk using System.IO i toppen.
 ![Exported image](Exported%20image%2020251104230903-0.png)  

Kode til at tjekke om en fil eksistere og hvis ikke til at oprette en:

![Exported image](Exported%20image%2020251104230905-1.png)  

Når man har noget der implementerer IDisposable kan man bruge keywordet using da den automatisk vil lukke for streamen i stedet for at man skal have en finally blok til at gøre det.  
Eksempel:  
Det ud kommenterede bliver unødvendigt med using foran FileStream instantieringen.
 ![Exported image](Exported%20image%2020251104230906-2.png)  

# FilePath, Relative vs Absolute:
 
I C# (og programmering generelt) refererer relative og absolutte filstier til måder, hvorpå man angiver en fils placering i forhold til projektets filsystem. Lad os dykke ned i deres betydning og forskelle:
 
**Absolutte filstier:**

- En absolut filsti angiver en fils fulde placering i filsystemet, startende fra rodmappen (root directory) på computeren.
- Den er uafhængig af, hvor programmet kører fra.
- **Eksempel på Windows:**
    - C:\Users\MyUser\Documents\myfile.txt
- **Eksempel på Linux/Mac:**
    - /home/myuser/documents/myfile.txt
- **Fordele:**
    - Præcis og altid unik.
- Velegnet til filer, der ikke flyttes eller altid findes samme sted.
- **Ulemper:**
    - Kan gøre koden mindre fleksibel, da stien ikke ændrer sig afhængigt af, hvor programmet køres.
    - Kan være forskellig mellem udviklingsmiljøer og produktionsmiljøer.
 
**Relative filstier:**

- En relativ filsti angiver en fils placering i forhold til det aktuelle arbejdsbibliotek (current working directory).
- Typisk afhænger den af, hvor programmet startes fra.
- **Eksempel:**
    - Hvis programmet køres fra mappen C:\MyProject\, og man angiver en relativ sti som data\file.txt, refererer det til C:\MyProject\data\file.txt.
- Ofte bruges . (punktum) til at referere til den aktuelle mappe og .. (to punkter) til at gå en mappe op.
- **Eksempel:**
    - .\file.txt refererer til en fil i den aktuelle mappe.
    - ..\file.txt refererer til en fil i den overliggende mappe.
- **Fordele:**
    - Mere fleksibelt og bærbart, især til projekter, der skal flyttes mellem maskiner.
    - Lettere at håndtere i forhold til projektstrukturen.
- **Ulemper:**
- Kan være forvirrende, hvis det aktuelle arbejdsbibliotek ikke er tydeligt defineret.
- Kræver, at programmet kender den relative struktur på forhånd.
 
**Eksempler:**
 
**Absolut:**

![Exported image](Exported%20image%2020251104230908-3.png)  

**Relativ:**  
Bemærk at man er nød til at angive hvor på hukommelsen at filen ligger med baseDirectory variablen.

![Exported image](Exported%20image%2020251104230909-4.png)  

## Hvornår bruges hvad?

- Brug **absolutte stier**, når filens placering altid er fast (som en konfigurationsfil i et bestemt systembibliotek).
- Brug **relative stier**, når filerne er en del af projektet, og du ønsker fleksibilitet ved at flytte programmet mellem forskellige miljøer.