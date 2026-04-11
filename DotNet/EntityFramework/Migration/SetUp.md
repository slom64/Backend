```
Add-Migration Initialize
Update-Database
```

```c#
dotnet add package Microsoft.EntityFrameworkCore.Design

dotnet ef migrations add InitialCreate // This generates a **C# migration file** inside a `Migrations` folder.
dotnet ef database update // Creates tables in the database automatically ,No App.config required, uses `appsettings.json` connection string
```



