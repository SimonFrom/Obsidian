*P2P netværk er gode til mindre scenarier eller hvor sikkerhed og hastighed er en prioritet.
Multipoint er mere fleksible med hensyn til at tilføje flere enheder til netværket, men kræver mere vedligeholdelse og arbejde at sætte op.

# P2P:

- En Point-to-point forbindelse er en lukket forbindelse mellem to enheder.
- Data bevæger sig ikke på offentlige netværk og er derfor sikret godt imod trusler udefra.
- Dette gør også at man i mange tilfælde ikke vælger at kryptere sine data så voldsomt som man normalt ville. Dette kommer selvfølgelig også an på hvilke data der bliver sendt.

Mest brugt i løsninger hvor high bandwidth og low latency er vigtigt. P2P har også et minimalt packet loss.

Dette sikres ved at den rute som dataen skal igennem altid vil være den samme da det er lukket, på offentlige netværk kan data blive routet en anden vej end den hurtigste.

Definition på en Point to point forbindelse:
- *En linje imellem to enheder som er en unicast forbindelse, som betyder at det er en dedikeret forbindelse mellem kun de to enheder og sikre at de har adgang til hele båndbredden og udelukkende bruges til at sende data packets.

# Multi point:

- Multi point forbindelser har en transmitter og flere receivers.
- Data kan derved sendes til flere enheder på samme tid.
- Afhængig af konfiguration kan flere enheder sende data på samme tid.

Definition på en multipoint forbindelse.
- *Multi point forbindelser er flere enheder der deler en forbindelse til netværket. Dette betyder at de deler kapaciteten midlertidigt. Data packets bliver sendt til alle enheder der er forbundet på netværket og modtagerne læser adresse feltet på data packet og bestemmer om det skal sendes videre i software eller slettes.

# Forskelle mellem de to typer:

- Mængden af forbundet enheder.
	- P2P vil altid være et link mellem to enheder.
	- Multi point vil være flere enheder der deler et link.

- Kanal kapacitet:
	- P2P forbindelser har altid fuld adgang til kanal kapaciteten.
	- Multi point deler kapaciteten midlertidigt mellem enheder.

- Receiver og Transmitter.
	- P2P har en receiver og en transmitter.
	- Multi point har en transmitter og flere receivere.

- Egnede scenarier.
	- P2P er godt hvis man er afhængig af en hurtig og sikker forbindelse.
	- Multi point er mere fleksible, men langsommere og mindre sikre.

- Kompleksitet.
	- P2P er simple og mindre modtagelige overfor interferens da der kun er 2 enheder/signaler.
	- Multi point kan være mindre stabile på data transmission, konflikter og kommunikations effektivitet.