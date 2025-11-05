## 1. NAT (Network Address Translation)

Beskrivelse:
Gæsten får adgang til internettet gennem værtsmaskinens IP. VirtualBox fungerer som router/NAT-gateway.

Fordele:

- Internetadgang virker uden konfiguration.

- Gæsten er isoleret fra værtsnetværket (øget sikkerhed).

- Kræver ingen ændringer på værtsnetværket.

Ulemper:

- Andre maskiner (værten eller andre gæster) kan ikke tilgå gæsten direkte.

- Ikke egnet til servere eller tjenester, der skal være tilgængelige udefra.

- Begrænset netværksfunktionalitet (ingen indgående forbindelser som standard).

## 2. NAT Network

Beskrivelse:

Ligner NAT, men flere gæster kan dele det samme virtuelle netværk og kommunikere indbyrdes.

Fordele:

- Gæster har internetadgang.

- Gæster kan kommunikere med hinanden.

- God balance mellem isolation og funktionalitet.

Ulemper:

- Værten kan ikke direkte tilgå gæsterne uden port forwarding.

- Kræver lidt opsætning (skal oprette et "NAT Network" i VirtualBox).

## 3. Bridged Adapter

Beskrivelse:

Gæsten opfører sig som en rigtig maskine på samme fysiske netværk som værten. Den får sin egen IP fra netværkets router.

Fordele:

- Gæsten kan kommunikere direkte med værten og andre fysiske maskiner.

- Gæsten kan bruges som server (kan tilgås udefra).

- Internetadgang fungerer normalt, hvis netværket tillader det.

Ulemper:

- Mindre sikkert – gæsten er eksponeret direkte på netværket.

- Kræver, at det fysiske netværk tillader flere IP-adresser (ikke altid tilladt på fx campusnet).

- Kan give problemer på Wi-Fi (især med visse drivere).

## 4. Internal Network

Beskrivelse:

Et helt isoleret virtuelt netværk, kun til kommunikation mellem gæster i samme interne netværk.

Fordele:

- Total isolation – ingen adgang til værten eller internettet.

- Godt til test af netværksopsætninger og simulering af netværk uden risiko.

- Simpelt at sætte op mellem flere gæster.

Ulemper:

- Ingen internetadgang.

- Værten kan ikke kommunikere direkte med gæsterne.

- Skal bruges sammen med fx NAT for at kombinere internet og intern kommunikation.

## 5. Host-Only Adapter

Beskrivelse:

Gæsten og værten deler et privat virtuelt netværk, men ingen direkte internetforbindelse.

Fordele:

- Gæst ↔ vært kommunikation fungerer.

- God til test, udvikling og filoverførsel mellem vært og gæst.

- Isoleret fra resten af det fysiske netværk (mere sikkert).

Ulemper:

- Ingen internetforbindelse (kan dog kombineres med NAT).

- Andre maskiner på det fysiske netværk kan ikke se gæsten.

## 6. Generic Driver (sjældent brugt)

Beskrivelse:

Bruger en tredjepartsdriver (fx UDP Tunnel eller VDE). Mest til avancerede setups.

Fordele:

- Kan bruges til meget specialiserede netværksopsætninger.

- Giver fleksibilitet til tilpassede scenarier (fx simulering af WAN-forbindelser).

Ulemper:

Kompleks opsætning.

- Ikke nødvendigt i de fleste brugsscenarier.

- Ikke altid stabilt eller understøttet.