https://github.com/AirPlate-ApS/AirPlateServer/pull/413
https://github.com/AirPlate-ApS/app/pull/374

De to her er til live filteret og valg af lyd notifikationer. Det ligger på dev og er testet. 

Derudover har jeg backfill og rangering af zoner, som ikke er helt færdigt eller gennemtestet.
Det der mangler er at:
- Der skal fjernes et UI label på intrusion log siden, som viser om en zone er backfilled.
- Lige nu er det et valg om man vil backfille, August ville bare gerne have det som standard når en zone blev oprettet og en aktiv dato var sat bagud i tiden, men det skal testes hvor meget stress det sætter på Influx og om det låser UI'et på en måde. Det burde det ikke, men man ved aldrig når det kommer op imod en rigtig situation. 
- Jeg har kun testet mod en mock version af Influx uden særligt meget data i. 
- Zone rangeringen er heller ikke testet, men skulle virke. Den tilføjer bare en kolonne med et tal og rykker om hvad der bliver ramt først i stedet for at rækkefølgen var oprettelses rækkefølgen som før.

Der er ikke lavet pr på de ting og er både frontend, backend og db migration.
Det hele ligger på feat/backfill-new-alarm-zones og migration hedder 2026-08-27-alarm-zones-sort-order.sql.
