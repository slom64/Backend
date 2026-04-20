### Direct user
```csharp
var userProfile = context.User
	.Where(user => user.Id == userId)
	.Select(user => new ProfileAddressDto()
	{
		FirstName = user.FirstName,
		LastName = user.LastName,
		PhoneNumber = user.PhoneNumber,
		
		Country = user.Address.Country,
		City = user.Address.City,
		Description = user.Address.Description,
		ZipCode = user.Address.ZipCode
	}).FirstAsync<ProfileAddressDto>().Result;
```