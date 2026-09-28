#### 3. Hoved feature - Zone backfill og prioritering
- **Database setup - Influx/Timeseries og MySQL:**
  Influx holder alle punkter som bliver logget og MySQL holder kun et repræsentativt punkt for hver flyvning. 
  Rå punkter streamer ind i influx, herefter kører der to jobs, time mæssigt og døgn baseret, som laver dem til komplette flyvninger med zoner og locations data og skriver dem til MySql og laver en række per flyvning. 
- Begge databaser har data som den anden ikke har. Influx har f.eks hele rækken af punkter som sensoren har logget fra dronen, men basalt set er det ikke logget som en flyvning, kun en bucket af punkter/measurements med nogle drone informationer.
- MySQL derimod, samler al informationen sammenlignet på enten MAC/Takeoff punkt(OcuSync droner), start og stop koordinat, men opbevarer kun et repræsentativt punkt for flyvningen. Det er disse 3 felter der fungerer som nøgler imellem de to databaser.
- Derfor er det også nødvendigt at kigge i begge databaser for at kunne få det fulde billede. MySQL for at få oplysninger om organisationens sensorer og alarm zoner og Influx for at at få den fulde rute som dronen bevægede sig i og kunne holde dette op mod alarm zoner.
- **Program flow**
  Samtidig med at der bliver logget bliver der også åbnet en WebSocket forbindelse til live visning af drone aktivitet. Der er dermed 2 grene som modtager information samtidigt.