
Et Polyalphabetic cipher (på dansk: polyalfabetisk chiffer) er en type substitutionskryptering, hvor man ikke bruger én enkelt alfabetisk erstatning som i et simpelt Caesar- eller monoalfabetisk chiffer, men flere forskellige alfabeter, der skiftes imellem under krypteringen.

**Grundidé:**

I stedet for at hver bogstav i klarteksten altid bliver erstattet af det samme bogstav (som i Caesar-chifferet), bruger man en nøgle, der bestemmer hvilken forskydning (eller hvilket alfabet) der skal bruges for hvert tegn.

Antag at man har en klartekst der er "Hello" som krypteres med nøglen "Key".
Nøglen gentages så man har samme antal karakterer som i klarteksten --> "KeyKe".
Karakterens indeks i alfabetet bruges dernæst til at shifte karaktererne i klarteksten, som i Cæsar metoden.
K = 10
E = 4
Y = 24

Så kryptering er som følgende:
H +10 → R
E +4  → I
L +24 → J
L +10 → V
O +4  → S

Krypteret tekst = RIJVS

**Fordel:**

- Sværere at knække end simple substitutioner, fordi samme bogstav i klarteksten kan blive til forskellige bogstaver i chifferteksten, afhængigt af positionen og nøglen.
- Bryder den klassiske frekvensanalyse, som nemt kan afsløre simple cifre.

**Ulempe:**

- Hvis nøglen er kort og gentages ofte, kan man stadig analysere mønstre og bryde chifferet (fx med Kasiski-testen).
- Hvis nøglen derimod er lige så lang som teksten og kun bruges én gang (one-time pad), bliver chifferet teoretisk ubrydeligt.