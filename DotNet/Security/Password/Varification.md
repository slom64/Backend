```csharp
[HttpPost]
public ActionResult<string> Login([FromBody] UsernamePasswordDto usernamePasswordDto)
{
	if (usernamePasswordDto.Email == null || usernamePasswordDto.Password == null)
		return BadRequest("Incomplete Credentials");
	if ((new PasswordHasher<User>()).
	VerifyHashedPassword(user, user.PasswordHash, usernamePasswordDto.Password) == PasswordVerificationResult.Failed
		|| usernamePasswordDto.Email != user.Email)
		return Ok("Wrong username or password");
	return Ok(GenerateToken(user));
}
```