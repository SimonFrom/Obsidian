1. Garmin FIT SDK til generere .fit filer med energi indtag til brug direkte på enheden. Dette virker ikke på alle enheder, men umiddelbart skulle de mest gængse modeller have muligheden. 
2. Ideelt set ender vi med en desktop app der kan have adgang direkte til enhedens filsystem og uploade filen. Webappen burde også kunne med File System Access API, men jeg har ikke kigget yderligere på det. Ellers er det manuelt "flyt fil til usb drev" metoden.
3. Dette vil også give mulighed for avanceret bruger/rolle styring, i og med at atleter og coaches skal have adgang til forskellige ting i samme system.
4. PDF generering burde være nemt nok at smide ind når vi har energi planen på plads.
5. Sende .fit fil og planer direkte fra webappen. 

### Out of scope, men ikke sindsygt at have i tankerne.
1. Sammenligning af trænings planer til planlægning af trænings ture for atleter hvis planer og område passer sammen.
2. Trække data direkte fra Garmin og TrainingPeaks. Dette kræver adgang og tilladelser fra Garmin, som umiddelbart er ret selektive med hvem der har API adgang, ergo får vi ikke adgang til eksamens projektet, men det er ikke utænkeligt at vi kan senere hen hvis vi arbejder videre med det.