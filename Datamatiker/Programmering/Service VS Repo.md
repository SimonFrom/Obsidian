Inden for softwarearkitektur er Repository Pattern og Service Pattern to almindeligt anvendte mønstre. Selvom de kan virke ens, har de forskellige formål og bruges ofte sammen for at skabe en mere struktureret og vedligeholdelsesvenlig kodebase.
 
Repository Pattern adskiller dataadgangslogik fra forretningslogik ved at abstrahere dataadgangen bag en repository-grænseflade. Dette gør det lettere at ændre eller udskifte datakilder uden at påvirke resten af applikationen.  
Service Pattern fokuserer på forretningslogik og organiserer operationer på data uafhængigt af, hvordan de lagres eller hentes. Tjenester fungerer som mellemled mellem brugergrænsefladen og dataadgangslaget, hvilket skaber en mere modulær og overskuelig kode.  
Disse mønstre bruges ofte sammen: Services håndterer forretningslogikken, mens repositories står for datahåndtering. Dette gør systemet mere fleksibelt, vedligeholdelsesvenligt og skalerbart.
 
**Fordele og ulemper**

# Repository Pattern

**Fordele:**  
✔️ Adskiller dataadgang fra forretningslogik, hvilket øger fleksibilitet.  
✔️ Lettere at teste, da dataadgang kan mockes.  
✔️ Understøtter flere datakilder uden at ændre forretningslogikken.  
✔️ Gør koden mere struktureret og vedligeholdelsesvenlig.
 
**Ulemper:**  
❌ Kan føre til unødvendigt ekstra lag, hvis det ikke er nødvendigt.  
❌ Over-engineering kan ske i små projekter.  
❌ Kræver mere kode og kompleksitet i simple CRUD-operationer.
 
# Service Pattern

**Fordele:**  
✔️ Adskiller forretningslogik fra dataadgang, hvilket forbedrer organisering.  
✔️ Fremmer genbrug af kode og Single Responsibility Principle (SRP).  
✔️ Gør applikationen lettere at teste og vedligeholde.  
✔️ Muliggør nemmere udvidelse og ændringer af forretningsregler.
 
**Ulemper:**  
❌ Kan føre til meget komplekse service-lag i store systemer.  
❌ Risiko for mangelfulde domænemodeller, hvor forretningslogik er for spredt.  
❌ Overflødigt i meget små applikationer uden avanceret logik.
 
Begge mønstre er kraftfulde og kan, når de anvendes korrekt, forbedre softwarearkitekturen markant.