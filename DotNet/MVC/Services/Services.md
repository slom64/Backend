- In ASP.net there is no automatic way to make dependency injection for services, you should manually do `AddScope`. Or, you can use 3rd party tool like `Scrutor`.
- Make interface that another class implement thats for best practise like ( interface`IProductQueryService`, class `ProductQueryService`)

### Setting up
#### 3rd party
```csharp
dotnet add package Scrutor

// Add to program.cs
builder.Services.Scan(scan => scan
    .FromAssembliesOf(typeof(Program)) // Use typeof instead of generics
    .AddClasses(classes => classes.Where(type => type.Name.EndsWith("Service")))
    .AsMatchingInterface() // Matches IProductService to ProductService
    .WithScopedLifetime());
```
#### Manual
for each new service you should add it to scope for dependency injection
```csharp
builder.Services.AddScoped<IAuthService, AuthService>();
```

---
