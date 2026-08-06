
## Sammendrag:

## 03-08-2026
Jeg har løst merge konflikter for min feature fra sidste uge.

Jeg har også arbejdet videre med scanner gruppe funktionen. 
Der var lidt der skulle ændres fra sidste uge database mæssigt, gruppe_nummer gav ikke mening at have med alligevel, så det skulle slettes fra docker databasen. 

## 04-08-2026
Det viste sig at jeg havde misforstået eller ikke blevet helt korrekt informeret omkring kravene til grupperings funktionen. Jeg havde forstået at der skulle være et en til mange forhold, en scanner kan være med i flere grupper. Men det viser sig at det skulle være omvendt, en gruppe kan indholde flere scannere og en scanner kan kun tilhøre en gruppe. 
Det viser bare hvor vigtigt en god start med ordentlig kravs specificering er.
Her blev det heldigvis fanget hurtigt i en tidlig feedback og er en relativt lille feature og ændring, men jo større feature eller jo længere i processen man er jo værre bliver det.

## 05-08-2026
Ikke fylde db
Localstorage?
Idag opstod et problem. Med det database design/skema jeg havde startet med at lave var der faktisk ikke nogen måde at persistere navnene på grupperne, med mindre at der var nogle scannere i gruppen hvilket der ikke nødvendigvis er. 

Jeg prøvede at lave en løsning med at bruge localStorage da min tanke var at så længe det persisterede indtil dashboardet blev lukket var fint. Men der bliver åbenbart gemt en masse andre ting i localStorage og jeg har ikke nogen måde at skille mit fra det. 
Så løsningen blev at lave en tabel mere i databasen med id og gruppe navn. Så bliver navnet gemt der og kan genbruges og forsvinder uanset om der er indhold i.


## 06-08-2026
Sygedag