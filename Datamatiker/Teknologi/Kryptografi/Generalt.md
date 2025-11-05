# Perfect Secrecy:

- Betyder, at den krypterede besked (ciphertext) ikke giver nogen information om den oprindelige klartekst (plaintext) — selv hvis angriberen kender krypteringsmetoden.

Med andre ord: selv med uendelig computerkraft kan man ikke gætte klarteksten uden nøglen.

**For at opnå Perfect Secrecy skal følgende gælde:**

- Nøglen er fuldstændig tilfældig.
	→ Alle nøgleværdier er lige sandsynlige.

- Nøglen er lige så lang som beskeden.
	→ Hver del af beskeden har sin egen uafhængige nøglebit.

- Nøglen bruges kun én gang.
	→ Genbrug bryder hemmeligholdelsen.

**Selvom teorien er perfekt, er den meget upraktisk i virkeligheden:**

- Nøglen skal være lige så lang som beskeden.
- Nøglen skal deles sikkert mellem afsender og modtager.
- Nøglen må aldrig genbruges.
- Svært at implementere ved store datamængder.

# Message space:

Hvis man har en krypteret besked der er 20 karakterer lang, vil message space i dette tilfælde være alle mulige kombinationer af 20 karakterer.

# Key space:

Det samme som message, bare for nøglen i stedet.

# Ciphertext space:

Det samme som key og message, men med den krypterede besked.

# Pseudorandom numbers:

Et Pseudorandom Number Generator (PRNG) er en algoritme, der ligner tilfældighed, men i virkeligheden genererer tal ud fra en bestemt startværdi (seed).

Da PRNG’er er deterministiske (dvs. de følger en fast sekvens), vil rækkefølgen af tal gentage sig selv efter et stykke tid.
Dette gentagelsesinterval kaldes the period.
The period er længden af den sekvens af tal, som PRNG’en kan generere, før den begynder at gentage sig.

- Middle squares method:

Til at starte med vælge man helt tilfældigt nr, 123 f.eks.

Dette kaldes Seed. Dette ganges med sig selv:
	123 x 123 = 15129

Nu udskifter man seed tallet med de 3 midterste tal fra resultatet.
	123 --> 512

Nu ganger man det nye seed tal med sig selv.
	512 x 512 = 262144

Nu tager vi de sidste 4 - 1 tal og tilføjer til seed tallet i rækkefølge.
	512 -->  512214

Processen gentages så mange gange man har lyst til.