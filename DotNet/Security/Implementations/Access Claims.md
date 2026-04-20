
- Inside the controller, the .net core will inject the claims
```csharp
public async Task<ActionResult<ProfileAddressDto>> GetProfile()
{
	var userID = User.FindFirstValue(ClaimTypes.NameIdentifier); // <--- this one
	if (userID == null)
		return Unauthorized("something is wrong in token");
	var result = await authService.Profile(userID);
	if(!result.Succeeded)
		return Unauthorized(result.Error);
	
	return Ok(result.Data);
}
```