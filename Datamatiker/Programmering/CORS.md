Cross Origin Resource Sharing er en sikkerheds mekanisme der kontrollerer hvordan en webside fra et domæne, får adgang til ressourcer fra andre domæner, ved at tillade at sende specifikke HTTP headere. Dette udvider på den strengere Same Origin Policy (SOP).
CORS foretages preflight, for at sikre at serveren tillader anmodningen før data udveksles.

Kort sagt:
*CORS er en browser sikkerheds mekanisme, der kontroller om et website (origin) må lave HTTP requests til et andet website (en anden origin). Det er en lempelse af SOP (Same origin policy)*

En origin kan defineres som 3 ting:
1. Protokol (HTTP/HTTPS)
2. Domain (.com, .dk etc...)
3. Port (7833 f.eks)

## Sammenfatning

- CORS er **browser-baseret sikkerhed**
- Beskytter mod uautoriserede cross-origin requests
- Serveren bestemmer adgang via HTTP-headers
- Preflight (OPTIONS) bruges ved “farlige” requests
- CORS håndhæves **kun af browsere**