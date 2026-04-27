## Setup
### Add roles to user
```csharp
// Role "Customer" should exists in db before use this code.
await userManager.AddToRoleAsync(user, "Customer");
```
### Read User role
```csharp
var role = await userManager.GetRolesAsync(user);
```

---

## Controller
```csharp
[Authorize(Role = "Admin, SubAdmin")]
public endpoint()
```