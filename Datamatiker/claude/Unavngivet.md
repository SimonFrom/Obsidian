---
name: jobansoegning
description: Skriv en jobansøgning til Simon ud fra et jobopslag (link eller tekst). Vurderer hvordan Simon passer til stillingen, skriver ansøgningen i hans stil, gennemgår den for AI-formuleringer og gemmer den som Google Docs-dokument i en ny undermappe under "Job ansøgninger" på Drevet. Kan også lave en målrettet CV-version. Brug når brugeren skriver /jobansoegning, deler et jobopslag og vil søge, eller beder om en ny ansøgning.
---

# Jobansøgning

Arbejdsgang for en ny ansøgning. Alt foregår på dansk, medmindre jobopslaget er på engelsk.

## Kilder: hent fakta, opfind dem aldrig

Fakta om Simon står i hans CV og tidligere ansøgninger, ikke her. Læs dem hver gang, for de bliver opdateret.

| Hvad | Hvor |
|---|---|
| Mappen "Job ansøgninger" på Drevet | folder-id `1J3s0B5G_p3wVR0S5CduyrN40MIHKIE3m` (under "Datamatiker") |
| Generelt CV (Google Docs) | `1fNyozwUPy0VVYskr64W7l0QvrYblUzw3NObGVJktSDI`, "CV Simon From" |
| Målrettede CV'er og ansøgninger | én undermappe pr. stilling i "Job ansøgninger", fx "Ai udvikler i stålproduktion" |
| Lokale ansøgninger og noter | `~/Job ansøginger/` (bemærk stavefejlen i mappenavnet) |
| Kode til projekter | `~/Code/` (fx `FuelPace`, `praktik` = AirPlate, `cykel-service-v0`) |

Søg i "Job ansøgninger" (`parentId = '<folder-id>'` og undermapper) efter tidligere ansøgninger og målrettede CV'er, og brug den nyeste ansøgning som stilreference.

Hvis du mangler et faktum (rolle, periode, værktøj, hvem i gruppen der byggede hvad), så **spørg** i stedet for at gætte. Et forkert faktum i et CV eller en ansøgning er værre end et manglende.

Kendte faldgruber:
- FuelPace: Gemini bruges til at hente vejret via function calling mod Open-Meteo (`GeminiWeatherClient.cs`), ikke til at matche træningspartnere. Projektets CLAUDE.md er forældet på det punkt. Tjek koden, før du beskriver et projekts teknik.
- ROEQ: to ansættelser. Produktionstekniker (aug. 2023 – aug. 2024, Odoo, varemodtagelse) og studiemedhjælper (jun. – sep. 2025, automatiserede tests i Python). Hold dem adskilt.

## Forløb

1. **Læs jobopslaget.** Brug web-fetch-agenten til et link. Notér virksomhed, opgaver, krav, ønsker, frist, kontaktperson og om de beder om noget særligt i ansøgningen (fx "skriv at du fandt jobbet på Jobindex").
2. **Vurder hvordan Simon passer.** Læs CV'et fra Drevet og sammenlign med kravene. Giv en kort vurdering i chatten: hvor han passer godt (med konkrete eksempler), svage punkter og hvordan de kan dækkes. Vent på svar, før du skriver ansøgningen, hvis der er huller, han måske kan udfylde.
3. **Skriv ansøgningen** i hans stil (se nedenfor). Tilpas til netop de krav, opslaget nævner. Brug hans stærkeste konkrete eksempler for den stilling.
4. **Gennemgå teksten** for AI-formuleringer og grammatik (tjeklisten nedenfor), før du viser den.
5. **Opret en undermappe** i "Job ansøgninger" med et kort navn for stillingen, og gem ansøgningen der som Google Docs-dokument. Spørg først, hvis det er uklart, hvad mappen skal hedde.
6. **CV:** spørg, om han vil have en målrettet CV-version. Hvis ja, tag udgangspunkt i det generelle CV, og gem den nye version i samme undermappe. Det generelle CV ændres kun, hvis han beder om det.
7. **Afslut** med links til filerne og en kort liste over ting, han selv skal tjekke (pladsholdere, datoer, fakta du ikke kunne bekræfte).

## Google Drev og Docs

- Indlæs skill'en `anthropic-skills:google-workspace` og læs `references/docs.md`, før du opretter eller retter et Google-dokument.
- Opret nye dokumenter med Drive `create_file`, `contentMimeType: "text/html"` og `parentId` sat til undermappen. Brug `<h1>`/`<h2>`/`<h3>`, `<p>`, `<ul>`. Ingen tastede punkttegn.
- Efter oprettelse, i denne rækkefølge:
  1. **Skift til A4-størrelse.** Nye dokumenter er i Letter-format, og Drive kan ikke sætte sidestørrelsen ved oprettelse. Læs dokumentet med Docs `read_doc`, og send med `update_doc` en `updateDocumentStyle`-ændring: `{"updateDocumentStyle": {"documentStyle": {"pageSize": {"width": {"magnitude": 595.28, "unit": "PT"}, "height": {"magnitude": 841.89, "unit": "PT"}}}, "fields": "pageSize"}}`. Hvis connectoren afviser ændringen, så sig det til Simon og bed ham skifte under Filer → Sideopsætning.
  2. **Gør overskrifterne mindre** (gælder især CV'er, hvor Google Docs' standardoverskrifter fylder meget): H1 20 pt, H2 13 pt, H3 12 pt, og stram afstanden før og efter overskrifterne. Læs dokumentet igen først, da indekser og revision er ændret efter trin 1.
  3. **Eksportér som PDF og tjek siderne.** Et CV bør holde sig til 2 sider, en ansøgning til omkring 1.
- Simon redigerer ofte i dokumenterne samtidig. Brug altid `requiredRevisionId` fra en frisk læsning. Hvis en skrivning afvises to gange, så stop og sig det i stedet for at prøve igen.
- Slet eller flyt aldrig filer, han ikke har bedt om.

## Stil

Simons egne ansøgninger er skrevet i et roligt, almindeligt dansk. Følg det:
- Brevform med "Kære <navn>". Hvis opslaget ikke nævner en kontaktperson, så brug "Kære <virksomhedens navn>" eller "Kære rekrutteringsansvarlige". Afslut med "Med venlig hilsen / Simon From". Ingen mellemoverskrifter i selve ansøgningen.
- Korte, konkrete afsnit: hvad han har lavet, med hvilke værktøjer, og hvad det lærte ham, koblet til stillingens krav.
- Han nævner ærligt, hvad han ikke har prøvet, og at han gerne lærer det. Ingen overdrivelser.
- Omkring én side.
- Afslut med hvornår han kan starte, at han gerne kommer til samtale, og GitHub-link (github.com/SimonFrom).

## Tjekliste for AI-formuleringer

Skriv om, hvis teksten indeholder:

- Overskrifter eller slogans i brevet ("Forretningen først", "AI i praksis").
- Punchlines og aforismer ("Når noget fejler, skal man kunne se hvorfor").
- Metaforer, hvor et almindeligt ord kan bruges ("Python har fulgt mig").
- "Ikke X, men Y"-konstruktioner ("…og ikke kun i chat", "…ikke med teknologien").
- "Fra X til Y"-opremsninger ("fra droneovervågning til sportsernæring").
- Kolon-indledninger i stil med "…det jeg helst vil lave: at …".
- Lister med præcis tre ting for rytmens skyld.
- Tomme forstærkere ("virkelig", "reelt", "særdeles", "solid", "robust").
- Tankestreger midt i sætninger. Brug komma eller punktum.
- Fagudtryk, en ikke-teknisk læser ikke kender, hvor et almindeligt ord kan bruges ("NDA" → "fortrolighedsaftale").

Tjek også dansk grammatik: "3. semester" med punktum, sammensatte ord i ét ord ("billetløsninger"), ingen komma før verbet ("min gruppe og jeg laver"), rigtig tid (fremtid om ting, der ikke er sket endnu).
