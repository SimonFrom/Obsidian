# TCP – 12 simple idéer (opsummering)

Videoen “TCP – 12 simple ideas to explain the Transmission Control Protocol” gennemgår 12 centrale koncepter som tilsammen giver en forståelse af hvordan TCP virker. Her er de 12 idéer forklaret på dansk:

1. Sekvensnumre og acknowledgments:

TCP bruger sekvensnumre til at holde styr på hvilke bytes der er sendt, og acknowledgment-numre til at signalere hvor langt modtageren er nået med at modtage data.

2. Sekvens‑ og ACK‑numre sporer bytes:

Ikke bare “pakker” men konkrete byte-positionsnumre – hvilket gør TCP i stand til at opdage manglende bytes, duplikater osv.

3. Retransmission Timer:

Der er en timer der overvåger om en sending ikke bliver bekræftet inden for en vis tid – så gen-sendes de manglende data.

4. Forsinkede acknowledgments (Delayed ACKs):

Modtageren kan vente lidt med at

sende ACK – nogle gange kombinerer ACK for flere pakker, i stedet for straks for hver enkelt. ACK’erne er kumulative.

5. Vinduesstørrelse og “Bytes in Flight”:

“Window size” er hvor mange bytes der må være sendt men ikke bekræftet endnu (“in flight”). Det sætter grænser for hvor meget data afsenderen må sende før den venter på ACK.

6. Vinduesstørrelse, headerfelter, og flowkontrol:

Modtageren reklamerer med sin modtagevinduets størrelse i TCP headeren så afsenderen ved hvor meget den må sende. Det hjælper med flow control.

7. TCP er bidirektionel:

Begge parter (klient og server) sender og modtager – hver part har sit eget sekvens‑ og ACK‑nummer. Kommunikationen kan foregå begge veje samtidigt.

8. Initielle sekvensnumre (ISNs) er tilfældige:

For sikkerhed og for at undgå visse angreb, vælges initial sequence numbers tilfældigt.

9. Three‑Way Handshake:

Den proces TCP bruger for at etablere forbindelse: SYN → SYN‑ACK → ACK.

- SYN = Synchronize

Et flag i TCP-headeren. Klienten sender et TCP-segment med SYN-flaget sat for at sige “jeg vil gerne starte en forbindelse og synkronisere sekvensnumre”.

- SYN-ACK = Synchronize + Acknowledge

Serveren svarer med et TCP-segment hvor både SYN- og ACK-flag er sat. Det betyder “jeg accepterer din anmodning og sender mit eget sekvensnummer tilbage”.

- ACK = Acknowledge

Klienten sender et sidste segment med ACK-flaget sat for at kvittere, at den har modtaget serverens SYN-ACK.

10. To metoder til at lukke forbindelse:

FIN og RST: FIN bruges til en “pæn” lukning (graceful) hvor begge parter afslutter ordentligt; RST bruges når en part vil afbryde forbindelsen abrupt eller ved fejl.

11. FIN-flag og den “fire-vejs” lukning:

En fuld, ordentlig afslutning kan kræve fire segmenter: en part sender FIN, den anden sender ACK, sender også FIN, modtager sender ACK.

12. RST-flag som brat terminering:

RST (reset) terminerer forbindelsen med det samme; bruges typisk hvis der er fejl, eller hvis en side modtager et uventet segment den ikke kan håndtere.

Eksempler: Når du browser en hjemmeside eller laver HTTP‑request bruges TCP’s handshake. Ved tabte pakker genudsender TCP dem. Ved lukning af forbindelse bruges FIN/ACK eller RST.