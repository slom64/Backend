```csharp
//services
builder.Services.AddIdentity<User, IdentityRole>(options =>
{
	options.SignIn.RequireConfirmedAccount = true;
})
	.AddEntityFrameworkStores<ApplicationDbContext>()
	.AddDefaultTokenProviders();
///// context
public class ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) 
    : IdentityDbContext<User,IdentityRole,string>(options)
{
	 public DbSet<User> User { get; set; }
}
/// Logic

public class AuthService(ApplicationDbContext context,
	 IConfiguration configuration, UserManager<User> userManager) : IAuthService
{
	public async Task<ServiceResponse<User>> RegisterAsync(EmailPasswordDto emailPasswordDTO)
	{

		// If the email is already used return null.
		if (await userManager.FindByEmailAsync(emailPasswordDTO.Email) != null)
			return ServiceResponse<User>.Failure(new List<string>() { "Email is already in use" });
		
		// Use transactions
		using var transaction = await context.Database.BeginTransactionAsync();
		try {
			var user = new User
			{
				Email = emailPasswordDTO.Email,
				UserName = emailPasswordDTO.Email
			};
			var result = await userManager.CreateAsync(user, emailPasswordDTO.Password);

			// Failed to create the User and password in database
			if (!result.Succeeded)
			{
				return ServiceResponse<User>.Failure(result.Errors.Select(e => e.Description));
			}
			
			
			result = await userManager.AddToRoleAsync(user, "Customer");
			// Faild to add the role
			if (!result.Succeeded)
			{
				await transaction.RollbackAsync();
				return ServiceResponse<User>.Failure(result.Errors.Select(e => e.Description));
			}
			await transaction.CommitAsync();
			return ServiceResponse<User>.Success(user);
		}
		catch (Exception ex) 
		{
			await transaction.RollbackAsync();
			return ServiceResponse<User>.Failure(new List<string>() { ex.Message });
			
		}
	}
}
```