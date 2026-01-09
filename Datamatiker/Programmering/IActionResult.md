Returtypen for en action metode i ASP.NET Core MVC.
Den beskriver **hvad** der skal sendes retur til klienten, ikke bare **hvilken datatype**.

Helt kort kan man sige:
	*IActionResult == et HTTP svar*
	
IActionResult bruges til at repræsentere et HTTP-svar i ASP.NET Core.  
Det gør det muligt at returnere forskellige HTTP-statuskoder og svarindhold fra samme action-metode, afhængigt af udfaldet af forretningslogikken.

Hvis man bare returnerede et objekt på denne måde:
```
public Product GetProduct()
{
    return product;
}
```

Vil man f.eks ikke kunne returnere:
- 404 NotFound
- 400 BadRequest
- 201 Created
- headers
- redirects

Istedet kan man med IActionResult:
```
public IActionResult Get(int id)
{
    var product = _repo.Get(id);

    if (product == null)
        return NotFound();

    return Ok(product);
}
```

Returnere status koder med tilbage:
- 404 NotFound
- 200 Ok
- etc...