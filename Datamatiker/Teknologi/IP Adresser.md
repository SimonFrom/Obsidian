*Hvor MAC adresser er fysiske adresser tildelt af OEM fabrikanter og ikke kan ændres, er IP logiske adresser der bliver tildelt og kan ændres og er mere fleksible.

- På to netværk forbundet til en fælles router kan PC 1 på netværk 1 sende en frame med modtager og afsenders MAC, CRC, data og modtager og afsenders IP adresser. PC 1 kan via PC 2's IP adresse se at det ikke er samme netværk og vil derfor sende til routerens MAC adresse.
- Når denne frame når routeren vil routeren fjerne MAC, CRC og kun stå tilbage med data og IP adresser, kaldet IP packet.
- Udfra IP adressen vil routeren så tilføje sin egen MAC og modtagers MAC adresse og CRC og sende til PC 2.

IPV4 adresser - 192.168.1.1:

- 32 bit logisk adresse der bliver tildelt til en netværks enhed.
- Skrevet i decimal format med fire oktetter.
- En oktet er en samling af 8 enheder. Her betyder det at alle tal i adressen kan fremstilles med 8 binære bits.

En adresse med 192.168 er en privat adresse til intern kommunikation der ikke kan routes over internettet.

Prøver man at sende en frame med privat ip, vil det resultere i en dropped packet.

For at sende fra en privat ip bruger man NAT (Network Address Translation).

## RFC 1918 addresses:

Class A:
- 10.0.0.0 til 10.255.255.255
- Subnet: (10.0.0/8)
- Typisk brugt i store virksomheder

Class B:
- 172.16.0.0 til 172.31.255.255
- Subnet: (172.16.0.0/12)
- Typisk brugt i mellemstore virksomheder

Class C:
- 192.168.0.0 til 192.168.255.255
- Subnet: (192.168.0.0/16)
- Typisk brugt i private hjem eller mindre virksomheder

## Loop back addresses:

127.0.0.0/8 blokken er reserveret og kan ikke tildeles en enhed.
Den mest brugte er 127.0.0.1.
Sender man en packet til denne vil den blive modtaget af ens egen pc.

## Public vs private IP:

For at kunne forbinde til internettet skal man have en public IP adresse. Denne bliver tildelt af service udbyderen.
Routeren tildeler via DHCP til at tildele private IP adresser til enheder på netværket.
I teorien kunne alle enheder have deres egen public IP, men da der er et begrænset antal af disse til rådighed, omkring 4,3 milliarder eller 2^32, skabte man private IP adresser som gør det muligt for flere enheder at dele en public IP.