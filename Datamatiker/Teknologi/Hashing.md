Hashing kan beskrives som en algoritme som tager data af hvilken som helst størrelse og laver det om til data på en bestemt størrelse

**Hvad er hashing?**

Hashing er en algoritme, der omdanner data af vilkårlig størrelse til en fast længde (f.eks. 128-bit). Det bruges til hurtigere at kunne sammenligne store datamængder.
Et hash er ikke reversibelt, i modsætning til kryptering — du kan altså ikke få originaldataen tilbage ud fra et hash.
En hashing algoritme bør være "Fast on software, slow on hardware".

**Egenskaber ved kryptografiske hash-funktioner**

En god hash-funktion skal:
- Være let at beregne for en given besked.
- Gøre det umuligt at finde en besked ud fra et givent hash (pre-image resistance).
- Gøre det umuligt at ændre beskeden uden at ændre hash-værdien.
- Gøre det umuligt at finde to forskellige beskeder med samme hash (collision resistance).

**Hash-funktioner skal derfor være modstandsdygtige mod:**

- Collisions – to beskeder giver samme hash.
- Pre-image attacks – finde en besked ud fra et hash.
- Second pre-image attacks – finde en anden besked med samme hash.

**Moderne hashing-algoritmer**

- MD-5 → Hurtig, men kryptografisk usikker (sårbar over for collisions).
- SHA-1 → 160-bit hash, designet af NSA, men ikke længere sikker efter 2010.
- SHA-2 → Familie af sikre algoritmer: SHA-256, SHA-512, SHA-224, SHA-384.
- SHA-3 → Den nyeste standard baseret på Keccak. Designet til at erstatte SHA-2 i fremtiden.

**Stærke passwords og angrebstyper**

Selv med god hashing skal brugerne have stærke passwords (mindst 8 tilfældige tegn, inkl. tal, symboler og store bogstaver).

**Angreb mod hash-lagrede passwords kan være:**

- Dictionary attack → Prøver almindelige ord, navne, årstal m.m.
- Brute force → Prøver alle mulige kombinationer.
- Rainbow tables → Forudberegnede databaser af hash-værdier for hurtig opslag.

Et moderne grafikkort (GPU) kan teste over 10 milliarder MD-5-hashes i sekundet.
Med 8-tegns passwords kan alle kombinationer gennemgås på ~2 dage i et GPU-cluster.
Tilføjer du blot ét tegn (9 tegn), tager det ~305 dage; 10 tegn → 106 år.
Dette viser, hvor meget password-længde betyder for sikkerheden.

**Hvorfor hasher vi passwords?**

Formålet er damage containment:
Hvis en angriber får adgang til databasen, skal det være svært og tidskrævende at gendanne brugernes rigtige passwords.
Brugere genbruger ofte samme password flere steder, så hashing beskytter mod kædereaktioner af kompromitterede konti.

**Krav til password-hashing**

Unik salt for hvert password (bruges kun én gang i hele databasen).
→ Forhindrer brute force og rainbow table-angreb på flere hashes samtidig.

Hurtig i software, men langsom i hardware – så distribuerede angreb bliver dyre og langsomme.

**Salt og Pepper**

- Salt = En unik, ikke-hemmelig værdi, som tilføjes passwordet før hashing.
	→ Sørger for, at identiske passwords får forskellige hashes.
	→ Gør rainbow tables og masse-angreb ubrugelige.
	→ Skal aldrig baseres på brugerdata.

- Pepper = En hemmelig nøgle, som bruges til at lave et HMAC (Hash-based Message Authentication Code).
	→ Forhindrer, at hash kan reproduceres uden nøglen.
	→ Dog begrænset effekt, hvis angriberen får adgang til serverens nøgle eller hukommelse.