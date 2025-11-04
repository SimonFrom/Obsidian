I C# er forskellen mellem værdi-typer og reference-typer følgende:

### Værdi-typer:

- Gemmer data direkte. Når du tildeler eller kopierer en værdi-type, kopieres selve værdien.
- **Opbevares på stacken**, hvilket gør dem hurtigere at tilgå.
- Eksempler: `int`, `float`, `bool`, `char`, `struct`, `enum`.
- Ændringer i én variabel påvirker ikke andre kopier af den.
 
### Reference-typer:

- Gemmer en reference til data, ikke selve dataene. Flere variabler kan referere til de samme data.
- Opbevares på heapen (referencen selv er på stacken).
- Eksempler: Klasser (`class`), arrays, `string.`
- Ændringer i data via én reference påvirker alle andre, der refererer til de samme data.
 
Eksempel med værdi type:

![int int b a b b fr en kopi af vrdien i a 18 ndrer ...](Exported%20image%2020251104230739-0.png)  

Eksempel med reference type:

![class Person public string Person personl personl ...](Exported%20image%2020251104230741-1.png)