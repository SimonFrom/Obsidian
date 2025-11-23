Dette er et mønster der siger at man bør dele applikationen op i flere komponenter, hvor hver komponent har et ansvar ala [[SOLID]]. 
Parent komponenten er ansvarlig for at opbevare og opdatere root state og 
child komponenterne står for at udføre en del af eller hele logikken.

![[Pasted image 20251119202956.png]]

Her vises at parent sender parameter videre til child som kommunikerer tilbage med EventCallback

## Struktur i praksis:


