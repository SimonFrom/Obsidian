Den logiske arkitektur organiserer softwareklasser i pakker (eller namespaces), delsystemer og lag. Det kaldes "logisk", fordi det ikke indebærer beslutninger om, hvordan elementerne distribueres på tværs af processer eller fysiske computere (det hører til i implementeringsarkitekturen).
 
Et lag grupperer klasser, pakker eller delsystemer, der er ansvarlige for en central del af systemet. Højere lag, som UI-laget, kalder tjenester fra lavere lag, men ikke omvendt. Almindelige lag i et objektorienteret system inkluderer:
 
- **Brugergrænseflade**
- **Applikationslogik og domæneobjekter** – objekter, der repræsenterer domænekoncepter, som en Salg-klasse, der opfylder krav som at beregne totaler.
- **Tekniske tjenester** – genanvendelige, applikationsuafhængige objekter, der yder teknisk support som databaseinterface eller fejllogning.  
I strikte arkitekturer interagerer lag kun med det lag, der ligger lige under, en designstil der ofte bruges i netværksprotokoller. Informationssystemer bruger dog typisk en mere fleksibel tilgang, hvor højere lag, som UI-laget, kan kalde både applikationslogik og tekniske tjenestelag.  
Selvom det ikke er et krav, er lagdelt arkitektur meget almindelig.

![Exported image](Exported%20image%2020251104231339-0.png) ![Exported image](Exported%20image%2020251104231340-1.png)