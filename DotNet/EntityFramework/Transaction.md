- A **Transaction** in ASP.NET Core is a way to ensure that multiple database operations are treated as a **single unit of work**. It follows the **ACID** principle: either all operations succeeds, or if one fails, the entire set of changes is undone (**rolled back**).

```csharp
public async Task<ServiceResponse<User>> RegisterAsync(EmailPasswordDto dto)
{
    // 1. Initial check (Read-only, doesn't need to be in the transaction)
    if (await userManager.FindByEmailAsync(dto.Email) != null)
        return ServiceResponse<User>.Failure(new List<string> { "Email is already in use" });

    // 2. Start the Transaction
    using var transaction = await context.Database.BeginTransactionAsync();
	
    try
    {
        var user = new User { Email = dto.Email, UserName = dto.Email };
		
        // Operation A: Create User
        var createResult = await userManager.CreateAsync(user, dto.Password);
        if (!createResult.Succeeded)
        {
            // No need to rollback yet, nothing was committed
            return ServiceResponse<User>.Failure(createResult.Errors.Select(e => e.Description));
        }
		
        // Operation B: Add Role
        var roleResult = await userManager.AddToRoleAsync(user, "Customer");
        if (!roleResult.Succeeded)
        {
            // Something went wrong! Roll back everything (removes the user created in Op A)
            await transaction.RollbackAsync();
            return ServiceResponse<User>.Failure(roleResult.Errors.Select(e => e.Description));
		}
		
        // 3. If we reached here, commit the changes to the database
        await transaction.CommitAsync();
        return ServiceResponse<User>.Success(user);
    }
    catch (Exception)
    {
        // 4. Critical failure (e.g., DB connection lost midway)
        await transaction.RollbackAsync();
        return ServiceResponse<User>.Failure(new List<string> { "An unexpected error occurred." });
    }
}
```