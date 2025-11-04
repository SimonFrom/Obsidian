## High-Level Software Design (HLD)

Fokus: Systemets overordnede struktur og arkitektur.

Formål: Vise hvordan systemet er opdelt i moduler, services eller komponenter.

Detaljeringsgrad: Grov – beskriver ikke implementeringsdetaljer.

Indhold:

- Sikkerheds foranstaltninger
- Hardware
- User interface - kun groft -> hvilke knapper skal der være? Ikke hvor de skal være!
- Interne interfaces der gør at teams kan samarbejde på programmet samtidigt
- Externe interfaces -
- Arkitektur:

- Monolithic

	- Programmet gør selv alt hvad det kan. Man er ikke afhængig af eksterne resurser eller andre ting.

- Client/Server

	- Client og server skilles ad, således at flere programmer/klienter kan tilgå serveren/databasen på samme tid

- Component based

	- En component-based arkitektur er en softwarearkitektur, hvor systemet er bygget op af komponenter – selvstændige, genanvendelige moduler med veldefinerede interfaces.
	- Tanken er, at hver komponent kan udvikles, testes og udskiftes uafhængigt af de andre, så længe den opfylder kontrakten (interface) over for resten af systemet.

- Server oriented

	- Minder meget om Client/Server. Logik og data ligger på serveren og klienterne er user interface.

- Data-Centric

	- En data-centric arkitektur er en arkitektur hvor data er det centrale element i systemet – alle komponenter, tjenester og processer er bygget op omkring adgang til, og behandling af, data.
	- I stedet for at funktioner eller services er kernen, er den fælles datakilde centrum for al kommunikation.

- Event-Driven

	- En event-driven arkitektur er et designmønster, hvor hændelser (events) er det centrale kommunikationsmiddel mellem systemets dele.
	- I stedet for at én komponent direkte kalder en anden (som i klassisk klient–server), udløser en komponent en event, og andre komponenter lytter efter og reagerer, hvis de er interesserede.

- Distributed

	- En distribueret arkitektur er et system, hvor funktionalitet og data er fordelt på flere maskiner/noder, der samarbejder via et netværk.
	- I stedet for at alt kører på én server (som i klassisk client–server) er det splittet op – men systemet opfører sig stadig som én samlet løsning for brugeren.

- Database - Hvilken type? Grov opstilling af tabeller og modeller

Output/artefakter:

- Arkitekturdiagrammer (f.eks. komponentdiagrammer, deployments).
- Systemets “blueprint”.

Målgruppe: Projektledere, arkitekter og udviklere, som skal forstå systemets struktur.

## Low-Level Software Design (LLD)

Fokus: De enkelte moduler/klasser og deres interne funktion.

Formål: Give udviklere en præcis specifikation for implementering.

Detaljeringsgrad: Fin – beskriver datastrukturer, algoritmer og metode-signaturer.

Design tilgange:

- Design to schedule

	- Tidsrammen bestemmer hvad og hvor meget der skal implenteres.

- Design to tools

	- Hvilke værktøjer man har eller nemt kan få bestemmer her. Man skal ikke opfinde komplekse algoritmer, men prøve at bruge noget der findes.

- Process-Oriented design

	- Kig på de processer som programmet skal udføre og lad dem bestemme.

- Data-Oriented design

	- Kig på den mængde data du skal bruge for at løse programmets funktion.

- Object-Oriented design

	- Afklar hvilke objekter og hvilke relationer disse skal have.

- Hybrid

	- Blanding af hvad man har brug for.

Indhold:

- Klassediagrammer, sekvensdiagrammer.
- Metoder, attributter, relationer mellem klasser.
- Algoritmer, fejlhåndtering og pseudokode.

Output/artefakter:

- Klassediagrammer med detaljer.
- Funktionsbeskrivelser/pseudokode.

Målgruppe: Udviklere, der skal kode modulerne.