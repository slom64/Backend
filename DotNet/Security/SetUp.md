```
add JwtBearer
```


```csharp
// User: Class should implement IdentityUser 
// ApplicationDbContext should implement :IdentityDbContext<User, IdentityRole, string>(options)
builder.Services.AddIdentity<User, IdentityRole>(options =>
{
	options.SignIn.RequireConfirmedAccount = true;
})
	.AddEntityFrameworkStores<ApplicationDbContext>()
	.AddDefaultTokenProviders();
	
builder.Services.Configure<IdentityOptions>(options => {
    options.User.RequireUniqueEmail= true;
});

// Add authentication and authorization using jwt.
builder.Services.AddAuthentication(options =>{
                options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
                options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
                options.DefaultScheme = JwtBearerDefaults.AuthenticationScheme;
            }).AddJwtBearer(options => {
                options.SaveToken = false;
                options.TokenValidationParameters = new TokenValidationParameters()
                {
                    ValidateIssuer = true,
                    ValidateAudience = true,
                    ValidateLifetime = true,
                    ValidateIssuerSigningKey = true,
                    ValidIssuer = builder.Configuration["AppSettings:Issuer"],
                    ValidAudience = builder.Configuration["AppSettings:Audience"],
                    IssuerSigningKey = new SymmetricSecurityKey(
                        Encoding.UTF8.GetBytes(builder.Configuration["AppSettings:Key"]!))
                };
            });


// All endpoints requires user authentication unless its annotated with [AllowAnonymous] 
builder.Services.AddAuthorization(options =>
                options.FallbackPolicy = new AuthorizationPolicyBuilder()
                .AddAuthenticationSchemes(JwtBearerDefaults.AuthenticationScheme)
                .RequireAuthenticatedUser()
                .Build()
            );



app.UseHttpsRedirection();
app.MapStaticAssets();
app.UseRouting();
 
app.UseCors();
app.UseAuthentication();
app.UseAuthorization();
```