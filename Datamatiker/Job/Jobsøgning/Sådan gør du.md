# Sådan gør du

Det, du selv skal gøre, når agenten søger job for dig.

## Første gang

- [ ] Slå farverne til i filtræet: **Indstillinger → Udseende → CSS-snippets**, og slå `jobsoegning` til.
- [ ] Timeren, der kører søgningen hver morgen, er ikke sat op endnu. Indtil da skal du starte den selv (se herunder).

## Start en søgning

- **I Claude Code:** skriv `/job-soegning`.
- **Fra terminalen:** kør `~/.local/share/job-soegning/run.sh`. Den kører uden at spørge om noget og skriver sin log i `~/.local/share/job-soegning/logs/`.

En kørsel tager 10-25 minutter. Den første kostede 4,82 dollar ved API-pris, fordi alle 104 opslag var nye. Senere kørsler er billigere, fordi der kun er få nye opslag.

## Ret i søgningen

Alt står i [[Søgeord og sider]]. Tekst mellem `%%` og `%%` bliver ignoreret, så du kan slå et søgeord fra uden at slette det.

- Søgeord med flere ord virker fint. Agenten sætter selv anførselstegn om dem.
- Jobnet og Computerworld kan ikke læses af agenten. Jobindex virker.

## Gennemgå nye opslag

1. Åbn [[Overblik.base]], og se fanen **Ikke sendt**.
2. Læs noten for et opslag. Under "Hvor jeg mangler noget" står det, du ikke opfylder, også hvis de beder om flere års erfaring.
3. Noter i den blå mappe `Remote og hybrid` er remote eller hybrid. Noter i den orange `På stedet` kræver, at du møder ind hver dag, eller også siger opslaget ikke noget om det.

Opslagene står også i [[Skal søges]], med link til note og udkast på hver linje.

## Udkast på Drevet

Udkastene ligger i en ny mappe under "Job ansøgninger" for hver stilling.

- Titlen begynder med **"Udkast - "**. Slet det ord, når du har læst ansøgningen igennem.
- Det, du selv skal udfylde, står med **grøn baggrund** som `[TJEK: …]`. Skriv det ind, eller slet det. Grønt må aldrig stå tilbage, når du sender.
- Hvis agenten har lavet et målrettet CV, ligger det i samme mappe. I noten står det, hvad der er ændret i forhold til dit generelle CV.
- Tjek, at ansøgningen holder sig til omkring én side.

## Få et udkast til et opslag uden udkast

Skriv ` · udkast mangler` sidst på linjen i [[Skal søges]]. Næste kørsel laver udkastet først, før den søger efter nye opslag. Der laves højst 3 udkast pr. kørsel.

## Når du har søgt

Gør én af to ting:
- sæt kryds i **sendt** øverst i noten, eller
- kryds linjen af i [[Skal søges]].

Overblikket opdaterer med det samme. Ved næste kørsel sætter agenten det andet sted og flytter noten til den grønne mappe `Søgt`.

Skriv gerne under linjen i Skal søges, hvordan og hvornår du søgte, fx `Sendt d. 24/11/26 via Jobindex`. Det rører agenten ikke.

## Frasorterede opslag

[[Frasorteret]] har en boks for hver kørsel. Den nyeste er foldet ud, og de ældre er foldet sammen. Kig især under **Gode match, men for langt væk**.

Vil du søge et frasorteret opslag alligevel, så skriv det ind i [[Skal søges]] i samme format som de andre linjer, og afslut med ` · udkast mangler`.

## Når noget ser forkert ud

- [[Kørselslog]] har én linje pr. kørsel med antal opslag og de problemer, agenten mødte.
- Skriv aldrig `?` som værdi i en notes properties. Obsidian kan ikke læse den. Brug `ukendt`.
