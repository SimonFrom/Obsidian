## Mål:
- Output som beskriver hvor langt folk er villige til at bevæge sig forskellige eventtyper.
- Markedsførings radius optimering.

### Krav:



#### Klasse:
**Event:**

- `locationCity` / `locationPostal` / `locationCountry` — event lokation
- `geoCode.lat` / `geoCode.lng` — event koordinater
- `locationName` — lokations navn
- `tickets[].price` — potentielt se om samme type events trækker mere i andre dele
- `maxAmount` — event størrelse
- `tags` / `seoSettings.tags` — hvis udfyldt, event type

**Transaction:**

- `ipLookup.lookup.city` — kunde by
- `ipLookup.lookup.region` — buyer landsdel
- `ipLookup.lookup.ll` — længde og bredde grader for mere præcis beregning, ikke super vigtig
- `ipLookup.lookup.country` — land, nok heller ikke super vigtig
- `ipLookup.lookup.area` — ip lookups præcisions score 
- `tickets[].amount` — antal per order
- `createdAt` — timing ifht event, købere længere væk køber måske tidligere
- `realPrice` — pris per order 


Features til beregning:

- Afstand mellem købers `ll` og eventets `geoCode` — din primære afstandsmetrik
- Køberregionkoncentration (% af købere fra samme region som venue)
- Tiltrækningsradius per eventtype (f.eks. 80% af købere inden for X km)
- Gennemsnitlig gruppestørrelse per køberregion
- Omsætning per region