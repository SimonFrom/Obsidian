GRASP står for General Responsibility Assignment Software Patterns og er et sæt af ni designprincipper eller mønstre, der hjælper softwareudviklere med at fordele ansvar i objektorienteret design. GRASP blev introduceret af Craig Larman og bruges især i forbindelse med UML og objektorienteret analyse og design (OOAD).
 
Her er en oversigt over de 9 GRASP-principper, med forklaringer og eksempler:
 
**1. Information Expert**  
Hvem skal have ansvaret? Den klasse, der har mest information til at udføre opgaven.
 
Eksempel: En Order-klasse skal beregne totalprisen, fordi den har adgang til ordrelinjer og priser.
 
**2. Creator**  
Hvem skal oprette et objekt? Den klasse, der:
 
- indeholder,
- bruger,
- eller har de nødvendige data til at oprette objektet.
 
Eksempel: En Order opretter OrderLine-objekter, fordi den indeholder dem.
 
**3. Controller**  
Hvem skal håndtere en systemoperation? En facade- eller controller-klasse, som repræsenterer systemet eller en del af det.
 
Eksempel: En OrderController håndterer en "createOrder"-operation fra brugergrænsefladen.
 
**4. Low Coupling**  
Formål: Reducere afhængighed mellem klasser, så ændringer i én klasse ikke påvirker andre.
 
Eksempel: En klasse bør ikke direkte afhænge af mange andre klasser.
 
**5. High Cohesion**  
Formål: Hold relateret funktionalitet samlet i samme klasse.
 
Fordel: Klassen bliver lettere at forstå, vedligeholde og genbruge.
 
**6. Polymorphism**  
Anvendelse: Brug polymorfi til at håndtere varierende adfærd.
 
Eksempel: En Shape-superklasse med draw()-metode, og underklasser Circle og Square, som implementerer metoden forskelligt.
 
**7. Pure Fabrication**  
Formål: Opret en klasse, der ikke repræsenterer en "ægte ting" i domænet, men som tjener designformål, fx for at opnå lav kobling og høj kohæsion, kunne også beskrives om hjælpeklasser.
 
Eksempel: En PersistenceManager, der håndterer lagring af objekter, selvom det ikke er en del af forretningsdomænet.
 
**8. Indirection**  
Formål: Indfør en mellemstation for at reducere afhængigheder.
 
Eksempel: Brug en controller, interface eller en Service klasse til at adskille UI fra forretningslogik.
 
**9. Protected Variations**  
Formål: Beskyt elementer mod variation ved at introducere abstraktioner og grænseflader.
 
Eksempel: Brug af interfaces, så du kan skifte implementeringen uden at påvirke resten af systemet.