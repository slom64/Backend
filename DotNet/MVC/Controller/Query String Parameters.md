
API
```csharp
[HttpGet("admin/product-{id}")]
public IActionResult GetProduct(FromRoute(Name = "id") string productId)
```