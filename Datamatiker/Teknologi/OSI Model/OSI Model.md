OSI står for "Open Systems Interconnection".

I praksis bruges [[TCP IP Model]] fremfor OSI. 

OSI modellen deler netværk kommunikation op i syv lag:
 
1. Physical - De fysiske forhold for netværket.
   Kabler, signaler osv
2. Data link - Kommunikation på lokalt netværk.
    MAC, Ethernet, Switches --> Hvem er næste punkt på ruten?
3. Network - Routing mellem netværk.
   IP adresser, Router, ping --> Hvor skal pakken hen?
4. Transport - Korrekt levering af data.
   **TCP** --> Pålidelig/Langsom, **UDP** --> Hurtig, upålidelig 
5. Session - Styrer sessioner mellem to systemer
   Åbner og lukker forbindelser, Session-ID'er --> Login, kurve osv.
6. Presentation - Sørger for at data kan forstås.
   Kryptering, Komprimering, Formatering --> Kan vi forstå hinanden?
7. Application - Det lag som brugeren interagerer med.
   HTTP, HTTPS, DNS --> Browsere, mail klienter, api'er.

For at kunne huske rækkefølgen kan følgende sætning bruges:  
**P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way

## Hurtig fejlsøgningsregel

> ❌ Ingen net → Lag 1–2  
> ❌ Kan ikke pinge → Lag 3  
> ❌ Forbindelse dropper → Lag 4  
> ❌ App virker ikke → Lag 7