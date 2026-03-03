En af de vigtigste steps i ML indebærer Data Collection.
Har man ikke god data at arbejde ud fra, kan man ikke regne med modellens output. 
Der kan være forskellig årsager til dårlig data:
- Menneskelige fejl i registrering
- Holdninger
- Eller at dataen simpelthen mangler
Har man manglende data bør man starte med at prøve at forstå hvorfor det mangler. Er der mønstre i hvad der mangler?

Der er forskellige måder man kan bearbejde eller erstatte manglende data på:
1. Fjerne alle rækker eller entiteter der har manglende værdier. Det siger sig selv at dette ikke er en ønsket mulighed da vi måske fjerner en del og gør dataen dårlig.
2. Tage de manglende værdier og sætte en fiktiv værdi ind. -1 eller NA f.eks
3. [[Imputation]]:
   En systematisk tilgang som erstatter værdien med den mest sandsynlige værdi.
   Dette kan i teorien give et komplet data sæt, men vi begynder også at digte data og ikke holde os til virkelighedens observationer.