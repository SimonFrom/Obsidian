Med DI kan man ved den oprindelige oprettelse af en instans styrer levetiden for services.
Det handler om hvornår en instans oprettes, deles og destrueres.


| Lifetime  | Oprettes                        | Deler instans?              | Typisk brug                      |
| --------- | ------------------------------- | --------------------------- | -------------------------------- |
| Transient | Hver gang den bruges            | Nej                         | Letvægts service                 |
| Scoped    | En gang pr request              | Ja, inden for samme instans | DbContext, repos                 |
| Singleton | En gang for hele appens levetid | Ja (Globalt)                | Cache, config, stateless service |
