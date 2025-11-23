
[[Low Level Design]]
[[Fokusområder]]

## Verificering:

- Har vi opstillet de rigtige kvalitetskriterier?
- Opfylder vi dem?
- Feedback fra PO

**Verificering** handler om at sikre, at systemet er **bygget korrekt** i forhold til de tekniske og funktionelle specifikationer.

Kort sagt: **Bygger vi systemet rigtigt?**  
(_I modsætning til "validering", der spørger: "Bygger vi det rigtige system?"_)

Verificering er altså en mere **teknisk og kvalitetsorienteret proces**, hvor man kontrollerer, om systemet virker, som det skal – ifølge de krav og specifikationer, der er sat op.

Verificering foregår typisk løbende under udviklingen og især ved implementering og test.

Det kan omfatte:

##### 1. **Teknisk test af funktioner**

[](https://github.com/bofl2002/Obsidian/blob/main/Verificering.md#1-teknisk-test-af-funktioner)

- Tjekker om alle funktioner **fungerer korrekt** og **uden fejl**.
    
- Fx: Kan brugeren logge ind? Gemmes data korrekt?
    

##### 2. **Systematisk gennemgang af krav**

[](https://github.com/bofl2002/Obsidian/blob/main/Verificering.md#2-systematisk-gennemgang-af-krav)

- Har vi **implementeret alle funktioner**, der er nævnt i kravspecifikationen?
    
- Matcher systemets adfærd de definerede krav?
    

##### 3. **Fejltest (bug testing)**

[](https://github.com/bofl2002/Obsidian/blob/main/Verificering.md#3-fejltest-bug-testing)

- Tester for tekniske fejl og uhensigtsmæssigheder i systemet.
    
- Både via automatiserede og manuelle tests.
    

##### 4. **Kvalitetssikring af kode og struktur**

[](https://github.com/bofl2002/Obsidian/blob/main/Verificering.md#4-kvalitetssikring-af-kode-og-struktur)

- Gennemgang af kodekvalitet og struktur.
    
- Brug af versionsstyring og kodegennemgange.
    

---

###### Eksempel: Verificering i et skoleprojekt

[](https://github.com/bofl2002/Obsidian/blob/main/Verificering.md#eksempel-verificering-i-et-skoleprojekt)

> Gruppen har lavet en webapp.  
> De gennemfører en række tests for at sikre, at:
> 
> - Alle knapper og links virker korrekt.
>     
> - Data bliver gemt rigtigt i databasen.
>     
> - Login og adgangskontrol fungerer som forventet.
>     
> 
> De sammenligner også deres løsning med den oprindelige kravspecifikation og bekræfter, at alle funktioner er implementeret som planlagt.

---

##### Metoder og værktøjer til verificering

[](https://github.com/bofl2002/Obsidian/blob/main/Verificering.md#metoder-og-v%C3%A6rkt%C3%B8jer-til-verificering)

|Metode|Beskrivelse|
|---|---|
|**Enhedstest (unit tests)**|Tester små dele af systemet (funktioner, klasser) hver for sig.|
|**Integrationstest**|Tester hvordan systemets dele arbejder sammen.|
|**Systemtest**|Tester hele systemet samlet.|
|**Check mod kravspecifikation**|Systematisk gennemgang af krav og tjek, om de er opfyldt.|
|**Testplan og testcases**|Foruddefinerede tests, der bruges til at kontrollere funktionalitet.|

---

##### ##Hvorfor er verificering vigtig?

[](https://github.com/bofl2002/Obsidian/blob/main/Verificering.md#hvorfor-er-verificering-vigtig)

- Sikrer, at systemet fungerer korrekt og uden fejl.
    
- Gør systemet mere stabilt og pålideligt.
    
- Mindsker risikoen for bugs, nedbrud og brugerfejl.
    
- Skaber tillid til, at systemet er teknisk velfungerende.
    

---

##### Verificering i rapport eller præsentation

[](https://github.com/bofl2002/Obsidian/blob/main/Verificering.md#verificering-i-rapport-eller-pr%C3%A6sentation)

Du kan fx skrive:

> **Verificering:**  
> Vi udførte en række funktionelle tests for at sikre, at systemet fungerer efter hensigten.  
> Alle krav fra kravspecifikationen blev gennemgået og afprøvet.  
> Fx blev login-funktionen testet med både korrekt og forkert input.  
> Vi fandt og rettede en fejl i søgefunktionen.  
> Efter test viste systemet sig at leve op til de tekniske krav, og der blev ikke fundet kritiske fejl.