```
dotnet add package FluentValidation.AspNetCore
```

```csharp
builder.Services.AddValidatorsFromAssemblyContaining<Program>();
```
---

### 1. Define the Validator

First, create a dedicated class for your DTO. This is where you define your custom rules.
```csharp
using FluentValidation;
using Ecommerce.Models.DTOs;

public class ProfileAddressDtoValidator : AbstractValidator<ProfileAddressDto>
{
    public ProfileAddressDtoValidator()
    {
        RuleFor(x => x.FirstName)
            .NotEmpty().WithMessage("First name is required")
            .MaximumLength(50).WithMessage("Name is too long");

        RuleFor(x => x.ZipCode)
            .Matches(@"^\d{5}$").WithMessage("Zip code must be exactly 5 digits")
            .When(x => !string.IsNullOrEmpty(x.ZipCode)); // Only validate if provided
            
        // Example of a custom cross-field validation
        RuleFor(x => x.Description)
            .NotEmpty()
            .When(x => x.Country == "International")
            .WithMessage("Description is required for international addresses.");
    }
}
```

### 2. Inject and Execute in the Service

In your `ProfileService`, you inject `IValidator<ProfileAddressDto>`. Instead of letting ASP.NET catch the error, you call it yourself.
```csharp
public class ProfileService : IProfileService
{
    private readonly ApplicationDbContext _context;
    private readonly IValidator<ProfileAddressDto> _validator; // Inject here
	
    public ProfileService(ApplicationDbContext context, IValidator<ProfileAddressDto> validator)
    {
        _context = context;
        _validator = validator;
    }
	
    public async Task<ServiceResponse<ProfileAddressDto>> UpdateProfile(string userId, ProfileAddressDto profile)
    {
        // 1. EXECUTE VALIDATION
        var validationResult = await _validator.ValidateAsync(profile);
		
        // 2. CATCH AND HANDLE
        if (!validationResult.IsValid)
        {
            // Extract the first error or join them all
            var errorMessage = validationResult.Errors.First().ErrorMessage;
            
            // Return using your existing ServiceResponse pattern
            return ServiceResponse<ProfileAddressDto>.Failure(errorMessage);
		}
		
        // 3. PROCEED TO BUSINESS LOGIC (Database updates, etc.)
        // ... (your existing update code)
    }
}
```