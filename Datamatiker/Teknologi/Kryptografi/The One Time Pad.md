The One-Time Pad (OTP) er den sikreste form for kryptering, der findes — teoretisk umulig at bryde, hvis den bruges korrekt.

Det er en særlig form for polyalphabetisk cipher.

**Grundidé:**

- En one-time pad bruger en helt tilfældig nøgle, der er:
- Nøglen skal være lige så lang som beskeden,
- Kun bruges én gang,
- Holdes fuldstændig hemmelig.
- Matematisk ubrydelig

Antag at sender og modtager har den samme nøgle "XMCKL".
Klartekst = "Hello"

Hvert bogstav konverteres til et tal (A=0, B=1, ..., Z=25), og man lægger nøglen til teksten indtil slutningen af indekset, derefter looper man rundt.

|   |   |   |   |
|---|---|---|---|
|Bogstav|Nøgle værdi|Shift|Resultat|
|H - 7|X - 23|7+23=30 → 4|E|
|E - 4|M - 12|4+12=16|Q|
|L - 11|C - 2|11+2=13|N|
|L - 11|K - 10|11+10=21|V|
|O - 14|L - 11|14+11=25|Z|

For at dekryptere trækker man bare nøglen fra i stedet.

**Ulemper:**

- Svært at bruge i praksis, fordi:
- Man skal kunne dele en kæmpelang tilfældig nøgle sikkert.
- Nøglen må aldrig genbruges.
- Begge parter skal have samme nøgle, og den skal destrueres efter brug.