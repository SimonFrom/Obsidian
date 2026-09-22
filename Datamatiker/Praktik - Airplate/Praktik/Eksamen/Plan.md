#### 1. Kort intro af AirPlate som virksomhed
AirPlate er en dansk ejet start up som arbejder i feltet omkring sporing og logging af drone aktivitet. 
De har udviklet deres egne scannere/sensorer til at spore aktiviteten og har udviklet deres egen web applikation og native app til visning og logging.
#### Tech baggrund:
Når en drone letter udsender den et RemoteID signal, som er en industri standard, som bliver sendt over WiFi/Bluetooth. Dette indeholder en MAC adresse og et sæt felter: 
- Længde og bredde grader
- Højde
- Hastighed
- Operatør ID
MAC adressen bruges som det primære ID igennem programmet efterfølgende.
Firmaet DJI har lavet deres egen standard, OcuSync. Her bliver der ikke sendt MAC, istedet sender den et takeoff punkt, dette punkt fungerer som ID igennem programmet istedet for.

#### 2. Hvad har jeg lavet
#### 3. Hoved feature - Zone backfill og prioritering
- Database setup:
  Influx holder alle punkter som bliver logget og MySQL holder kun et repræsentativt punkt for hver flyvning. 
  Rå punkter streamer ind i influx, herefter kører der to jobs, time mæssigt og døgn baseret, som laver dem til komplette flyvninger med zoner og locations data og skriver dem til MySql og laver en række per flyvning. 
- Begge databaser har data som den anden ikke har. Influx har f.eks hele rækken af punkter som sensoren har logget fra dronen, men i basalt set er det ikke logget som en flyvning, kun en bucket af punkter med nogle drone informationer.
- MySQL derimod, samler al informationen sammenlignet på 
#### 4. Refleksion over praktikken

