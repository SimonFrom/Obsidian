Simpel:
Kopier dette ind i cli'en i VS:

```
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore &&
dotnet add package Microsoft.AspNetCore.Identity.UI &&
dotnet add package Microsoft.EntityFrameworkCore &&
dotnet add package Microsoft.EntityFrameworkCore.Abstractions &&
dotnet add package Microsoft.EntityFrameworkCore.Design &&
dotnet add package Microsoft.EntityFrameworkCore.Sqlite &&
dotnet add package Microsoft.EntityFrameworkCore.SqlServer &&
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

Eller tilføj dette script til solutionen:
C:\Users\simon\OneDrive\Skrivebord\scripts\install-efcore-identity.ps1

Og kør dette i cli'en:
```
powershell -ExecutionPolicy Bypass -File install-efcore-identity.ps1
```
