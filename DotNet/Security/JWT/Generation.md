```csharp
public string GenerateToken(User user) 
{
		var claims = new List<Claim>
		{
			new Claim(ClaimTypes.NameIdentifier , user.Id.ToString()), // Add Id, it will help in database query.
			new Claim(ClaimTypes.Name, user.FirstName + " " + user.LastName),
			new Claim(ClaimTypes.Role,Enum.GetName<Role>(user.role)),
			new Claim(ClaimTypes.Email, user.Email)
		};

		var key = new SymmetricSecurityKey(
				Encoding.UTF8.GetBytes(configuration.GetValue<string>("AppSettings:Key")!)
			);

		var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
		var tokenDescriptor = new JwtSecurityToken(
			issuer: configuration.GetValue<string>("AppSettings:Issuer"),
			audience: configuration.GetValue<string>("AppSettings:Audience"),
			claims: claims,
			expires: DateTime.UtcNow.AddDays(1),
			signingCredentials: creds
			);

		return new JwtSecurityTokenHandler().WriteToken(tokenDescriptor);
	}
```