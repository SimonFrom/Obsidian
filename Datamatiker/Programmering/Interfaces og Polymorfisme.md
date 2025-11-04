# Interfaces:
 
[Getting started with interfaces and polymorphic behavior](https://vimeo.com/254573540)  
[BroCode interfaces](https://www.youtube.com/watch?v=RuhGv81tpoU&ab_channel=BroCode)  
[Interface Naming Guidelines](https://learn.microsoft.com/en-us/previous-versions/dotnet/netframework-1.1/8bc1fexb\(v=vs.71\)?redirectedfrom=MSDN)
 
- Interfaces i C# sørger for ensartethed.
- Interfaces implementerer ikke noget.
- Hvis en klasse arver fra et interface skal den afspejle interfacets metoder.
- Et interface bestemmer hvad der **skal** være i en klasse og en nedarvet klasser definer **hvordan** det skal gøres.
 
Navnekonventioner:

- Navngiv interfaces med substantiver eller substantivfraser, eller adjektiver der beskriver adfærd. For eksempel bruger interface-navnet IComponent et beskrivende substantiv. Interface-navnet ICustomAttributeProvider bruger en substantivfrase. Navnet IPersistable bruger et adjektiv.
- Brug Pascal-case. 
- \> Undgå forkortelser.  
    
- Navnet på interfacet skrives med stort i foran. Se nedenunder.
- Brug lignende navne, når du definerer et klasse/interface-par, hvor klassen er en standardimplementering af interfacet. Navnene skal kun adskille sig ved bogstavet I foran interface-navnet

\> Undgå forkortelser.   
Når man skriver et interface skriver man det på følgende måde:
 ![Exported image](Exported%20image%2020251104230547-0.png)  

Læg mærke til navnet op interfacet og at alle metoderne inde i skal implementeres i nedarvede klasser.
 ![Exported image](Exported%20image%2020251104230548-1.png)  

Med den nederste metode vil man også nemt kunne tilføje endnu en klasse som arver fra IMachine uden at skulle lave en ny metode til den nye klasse.
   

# Polymorfisme:

Polymorfisme kan koges ned til at en bil og plæneklipper kan begge to starte, men de gør det på forskellige måder.  
Dyr snakker, men en kat og en elefant lyder ikke ens.