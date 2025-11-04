Frames:

- Kan være op til 1500 bytes lang, 8 bits til en byte = 10000 binære tal.
- Bliver genereret og behandlet på netværks kort/adapter.
 
MAC adresse (Media Access Control):

- Kan findes i command prompt med 'ipconfig /all' og under physical address.
- Eksempel: D8-5E-D3-2F-5C-37
- De første 3 grupper af numre er OEM numre som er unikke for hver producent.
- De næste 3 er unikke id'er for dette netværks kort.
- Når en frame bliver genereret tilføjes modtager og afsenders MAC adresse til framen.
- CRC (Cyclic Redundancy Check) sørger at for dataen er komplet og ellers sendes den retur.