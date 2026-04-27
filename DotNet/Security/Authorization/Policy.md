- Its rules that are generated and set using User roles and claims. Ex, **AdminMalesOver10YearsOfExperience**.
```csharp
builder.Services.AddAuthorization(options =>
{
                options.FallbackPolicy = new AuthorizationPolicyBuilder()
                .AddAuthenticationSchemes(JwtBearerDefaults.AuthenticationScheme)
                .RequireAuthenticatedUser()
                .Build();
                
                
                options.AddPolicy("PolicyName", policy => policy.RequireClaim("Gender","Male"));
                options.AddPolicy("PolicyName", policy => policy.RequireAssertion(context => 
	                Int32.Parse(context.User.Claims.First(x => x.Type == "Age").Value) < 10
                ));
});

```


---
```csharp
[Authorize(Policy = "PolicyName")]
public endpoint()
```