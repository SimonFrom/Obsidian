*ARP er en protokol som kan finde MAC adresse udfra en IP adresse.

- Bliver brugt når en enhed vil kommunikere med en anden på et LAN netværk.
- Afsenderen bruger ARP til at oversætte IP adresser til MAC adresser
- Alle enheder på netværket ser denne besked, men kun den korrekte IP adresser svarer tilbage med dens MAC adresse.
- Kan findes i powershell med 'arp -a'.
- Kan være dynamic eller static - Dynamic bliver slettet ind imellem
- Med 'arp -s "ip adresse" "mac adresse" kan man lave et manuelt static entry som ikke bliver flushed