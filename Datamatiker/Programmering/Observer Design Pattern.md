[Observer Design Pattern](https://www.dofactory.com/net/observer-design-pattern)  
[LinkedIn Video](https://www.linkedin.com/learning/c-sharp-design-patterns-part-1/observer-pattern-overview?u=57075649)  
[Observer Pattern with interface](https://www.codeproject.com/Articles/796075/Implement-Observer-Pattern-in-NET-techniques)
 
Observer og Subject kan også omtales som henholdsvis Publisher og Subscriber
 
Observer-mønsteret er et adfærdsdesignmønster, der bruges, når et objekt (kaldet Subject eller Observable) skal informere flere andre objekter (kaldet Observers) om ændringer i sin tilstand, uden at de er tæt knyttet sammen.  
I Observer-mønsteret er Subject ansvarlig for at holde styr på alle Observers, og når der sker en ændring i Subject (f.eks. dataopdatering), underretter det automatisk alle registrerede Observers.  
Observer-mønsteret består typisk af følgende elementer:

- **Subject** (Observable):
    - Dette objekt har en intern tilstand og holder styr på alle Observers.
    - Det underretter alle Observers om ændringer.
    - Det tillader Observers at tilmelde sig eller afmelde sig opdateringer.
- **Observer**:
    - Denne interface/abstrakt klasse definerer metoden Update(), som kaldes, når der sker en ændring i Subject.
    - Observers lytter til Subject og reagerer på ændringer.
- **ConcreteSubject**:
    - En konkret implementering af Subject, der indeholder data og hændelseslogikken.
    - Når data i ConcreteSubject ændres, underretter det alle registrerede Observers.
- **ConcreteObserver**:
    - En konkret implementering af Observer, der reagerer på ændringer fra ConcreteSubject.
    - Denne klasse udfører den nødvendige handling, når den bliver underrettet om ændringer i ConcreteSubject.
   

Subject class:

![Exported image](Exported%20image%2020251104230931-0.png)  

Observer class:

![Exported image](Exported%20image%2020251104230932-1.png)  

Concrete subject class:

![Exported image](Exported%20image%2020251104230937-2.png)  

Concrete observer class:

![Exported image](Exported%20image%2020251104230938-3.png)