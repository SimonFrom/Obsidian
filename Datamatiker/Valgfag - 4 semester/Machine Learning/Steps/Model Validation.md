### Eksempel:

| Størrelse | Rum | Alder | Pris         |
| --------- | --- | ----- | ------------ |
| 120       | 4   | 10 år | 2.000.000 kr |
| 85        | 3   | 5 år  | 1.500.000 kr |
X = Str, Rum Alder --> Modellens input
y = Pris --> Det vi vil forudsige

| Variabel                                                                     | Navn                 | Bruges til                                                          |
| ---------------------------------------------------------------------------- | -------------------- | ------------------------------------------------------------------- |
| train_X                                                                      | Trænings features    | Det input modellen lærer af                                         |
| train_y<br>(De priser som modellen må lære af)                               | Trænings mål         | De rigtige faktuelle svar som modellen skal ramme tæt på            |
| val_X                                                                        | Validerings features | Input som modellen *ikke* har set                                   |
| val_y<br>(De priser som er gemt for modellen og skal bruges til at validere) | Validerings mål      | Det input som modellen *ikke* har set og som vi ønsker at forudsige |
| val_predictions                                                              | Forudsigelser        | Hvad modellen gætter på for val_X                                   |
### Huske regel:
- train_ = bruges til at træne modellen
- val_ = bruges til at teste modellens forudsigelser
- _ X = input (features/kolonner)
- _ y = output (den pris vi vil forudsige)