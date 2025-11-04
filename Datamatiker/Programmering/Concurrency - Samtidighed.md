# Samtidighedskontrol

Når flere brugere eller processer prøver at tilgå de samme data på samme tid, skal man styre samtidighed for at undgå konflikter (som at overskrive en andens ændringer).

To almindelige strategier er optimistisk samtidighed og pessimistisk samtidighed.

# Optimistisk samtidighed

Idé: Man antager, at konflikter er sjældne. Alle må læse og ændre data frit, men systemet tjekker for konflikter, når der gemmes.

Sådan fungerer det:

- Bruger A læser en post.
- Bruger B læser den samme post.
- Begge laver ændringer.
- Når A gemmer, opdateres posten.
- Når B gemmer, opdager systemet, at posten er ændret (via versionsnummer, tidsstempel eller hash) og afviser eller beder om sammenfletning.

Mekanisme:

Typisk ved hjælp af en versionskolonne eller række-tidsstempel i databasen. Opdateringen sker med WHERE Version = @Version. Hvis ingen rækker opdateres, er der sket en konflikt.

Fordele:

- Ingen låse – meget skalerbart.
- Godt til læsetunge systemer med få konflikter.
- Brugere blokerer ikke hinanden, mens de redigerer.

Ulemper:

- Man kan få fejl ved gemning (“nogen andre har ændret dette”), så man skal håndtere gentagelser eller konflikthåndtering i UI’et.
- Ikke godt til data, der opdateres meget hyppigt.

# Pessimistisk samtidighed

Idé: Man antager, at konflikter er sandsynlige. Systemet låser data, når de læses, så andre ikke kan ændre dem, før man er færdig.

Sådan fungerer det:

- Bruger A læser en post og får en lås.
- Bruger B forsøger at læse eller skrive til posten, men bliver blokeret, indtil A frigiver låsen.
- Bruger A gemmer eller annullerer, og låsen frigives.

### Mekanisme:

Brug af databaselåse eller transaktioner med SELECT … FOR UPDATE.

Andre er tvunget til at vente (eller fejle), indtil låsen frigives.

Fordele:

- Ingen konflikter ved gemning – systemet sørger for, at kun én kan skrive ad gangen.
- Velegnet til data med høj konfliktgrad eller stor værdi (fx bankkonti).

Ulemper:

- Låse mindsker skalerbarhed og gennemløb.
- Risiko for deadlocks, hvis det ikke styres ordentligt.
- Brugere kan blive blokeret, mens de venter på en lås.