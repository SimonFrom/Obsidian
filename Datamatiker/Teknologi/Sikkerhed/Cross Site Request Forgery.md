[Cross Site Request Forgery](https://www.youtube.com/watch?v=vRBihr41JTo&t=1s)
[CSRF Attacks](https://www.acunetix.com/websitesecurity/csrf-attacks/)


CSRF/Sea Surf/XSRF er et angreb som udnytter at brugeren er logget ind et andet sted og som uden deres viden sender et request et andet sted hen end det forventes. 
Det oprindelige site og indhold vil fungerer som normalt, men i baggrunden eller gemt bliver der godkendt noget andet. 

For at forhindre dette kan man bruge tokens til at checke om requesten skal godkendes.
