
- Inside the controller, the .net core will inject the claims
```csharp
// You need [Authorize] to access ClaimsPricipal "User" which do automatic DI. otherwise you can put it in constructor to do DI.
[Authorize]
public async Task<ActionResult<ProfileAddressDto>> GetProfile()
{
	// User object is from ClaimsPrincipal class.
	var userId = User.FindFirstValue(ClaimTypes.NameIdentifier); // <--- this one is LINQ, you can do the same thing here:-
	// Unpack ClaimTypes.NameIdentifier to its actual string value and you will get the same result as above.
	// This is good if you have custom claims.
	userID = User.Claims.First(jwtClaim => jwtClaim.Type == "nameidentifier").Value;
	if (userId == null)
		return Unauthorized("something is wrong in token");
	var result = await authService.Profile(userId);
	if(!result.Succeeded)
		return Unauthorized(result.Error);
	
	return Ok(result.Data);
}
```