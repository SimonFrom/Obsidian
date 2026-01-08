Bruges til at definere og starte flere containere samtidigt

#### Grundidé:
En YAML fil der beskriver:
- Services (Containere)
- Netværk
- Volumes (Persistens)
- Afhængigheder

Simpelt eksempel med web API og Database:

```
version: "3.9"

services:
  api:
    build: .
    ports:
      - "5000:8080"
    depends_on:
      - db

  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      SA_PASSWORD: "StrongPassword123!"
      ACCEPT_EULA: "Y"
    ports:
      - "1433:1433"
```
