#### 1. Kort intro af AirPlate som virksomhed
AirPlate er en dansk ejet start up som arbejder i feltet omkring sporing og logging af drone aktivitet. 
De har udviklet deres egne scannere/sensorer til at spore aktiviteten og har udviklet deres egen web applikation og native app til visning og logging.
#### 2. Hvad har jeg lavet
#### 3. Hoved feature
- Database setup:
  Influx holder alle punkter som bliver logget og MySql holder kun et repræsentativt punkt for hver flyvning. 
  Rå punkter streamer ind i influx, herefter kører der to jobs, time mæssigt og døgn baseret, som laver dem til komplette flyvninger med zoner og locations data og skriver dem til MySql og laver en række per flyvning. 
- 
- 
#### 4. Refleksion over praktikken

