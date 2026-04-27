```csharp
public async Task<ServiceResponse<string>> LoginAsync(EmailPasswordDto usernamePasswordDto)
        {
            
            var user = await userManager.FindByEmailAsync(usernamePasswordDto.Email);
			
            if ( user == null)
                return ServiceResponse<string>.Failure("Invalid Credentials");
			
			
            
            var state = new PasswordHasher<User>()
                .VerifyHashedPassword(
                    user, 
                    user.PasswordHash!, 
                    usernamePasswordDto.Password
                );
			
            if (state == PasswordVerificationResult.Failed)
                return ServiceResponse<string>.Failure("Invalid Credentials");
			
			
            return ServiceResponse<string>.Success(GenerateToken(user));
        }
```