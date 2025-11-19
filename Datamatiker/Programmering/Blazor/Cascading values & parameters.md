[Cascading Values](https://www.pragimtech.com/blog/blazor/blazor-multiple-cascading-parameters/)


En Blazor applikation består af komponenter. Man laver individuelle komponenter og samler dem sammen. Komponenter kan være nested i en anden komponent og den komponent kan være nested i en tredje komponent osv...

![[Pasted image 20251119211654.png]]

Når der skal sendes værdier og parametre mellem komponenter bruger man Cascading values. Man bruger den indbyggede Blazor komponent, *CascadingValue*.
Denne sørger for at værdien sendes videre i komponent træet.

![[Pasted image 20251119212223.png]]

*ParentComponent.razor*
```
@page "/pc" 
<h1 style="@Color">Parent Component Text</h1>

<CascadingValue Value="@Style"> 
	<ChildComponent> 
	</ChildComponent> 
</CascadingValue>

@code { 
	public string Style { get; set; } = "color:red"; 
}
```
CascadingValue bliver sat til værdien af "Style".

*ChildComponent.razor*
```
<h1 style="@ElementStyle">-Child Component</h1>

<GrandChildComponent></GrandChildComponent>

@code {
    [CascadingParameter]
    public string ElementStyle { get; set; }
}
```
ChildComponent kan tilgå den værdi ved at dekleare sin egen property, *ElementStyle* og dekorere den med *CascadingParameter* 

*GrandChildComponent.razor*
```
<h1 style="@ElementStyle">--Grand Child Component Text</h1>

@code {
    [CascadingParameter]
    public string ElementStyle { get; set; }
}
```
I samme stil kan en *GrandChildComponent* og tilgå det på samme måde.

Det er også muligt at sende to forskellige datatyper med.
```
<CascadingValue Value="@Style">
    <CascadingValue Value="@EmployeeAge">
        <ChildComponent></ChildComponent>
    </CascadingValue>
</CascadingValue>

@code {
    public string Style { get; set; } = "color:red";
    public int EmployeeAge { get; set; } = 25;
}
```

I dette tilfælde med den samme datatype, vil det være *BorderStyle* der er tættest på child og være det der bliver sendt videre.
```
<CascadingValue Value="@Style">
    <CascadingValue Value="@BorderStyle">
        <ChildComponent></ChildComponent>
    </CascadingValue>
</CascadingValue>

@code {
    public string Style { get; set; } = "color:red";
    public string BorderStyle { get; set; } = "border:1px solid red";
}
```

Har man flere værdier af samme datatype kan man navngive værdierne i Razor syntaksen.
```
<CascadingValue Value="@Style" Name="ColorStyle">
    <CascadingValue Value="@BorderStyle" Name="BorderStyle">
        <ChildComponent></ChildComponent>
    </CascadingValue>
</CascadingValue>

@code {
    public string Style { get; set; } = "color:red";
    public string BorderStyle { get; set; } = "border:1px solid red";
}
```
