- In ASP.NET there is framework services and application services. Framework services are automatically registered but application services should be explicitly registered in `program.cs`
- In ASP.NET Core, all service registrations happen in `Program.cs` using the `builder.Services` collection. This is where you define how your application’s dependencies are wired up - what interfaces map to which implementations.
```csharp
builder.Services.AddScoped<IOrderService, OrderService>();
builder.Services.AddSingleton<INotificationService, EmailNotificationService>();
builder.Services.AddTransient<IPaymentService, StripePaymentService>();
```
- You can also register services using lambda expressions for more control:
```csharp
builder.Services.AddScoped<IInvoiceService>(sp =>
{
    var logger = sp.GetRequiredService<ILogger<InvoiceService>>();
    return new InvoiceService(logger, "INVOICE_PREFIX");
});
```

---
### Separate regestering
- Split registrations into extension methods or separate static classes per feature or layer. This is a pattern you will very likely see in clean architecture / vertical slice architecture projects.
```csharp
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddApplicationServices(this IServiceCollection services)
    {
        services.AddScoped<IOrderService, OrderService>();
        services.AddScoped<ICustomerService, CustomerService>();
        return services;
    }
}
```
- And in `Program.cs`:

```csharp
builder.Services.AddApplicationServices();
```