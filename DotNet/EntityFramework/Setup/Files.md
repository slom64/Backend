create class
```c#
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
using Microsoft.Extensions.Configuration;
using System;
using System.IO;
using static Starter.Program;

namespace Starter
{
    public class AppDbContextFactory : IDesignTimeDbContextFactory<AppDbContext>
    {
	    IConfigure configuration;
        public AppDbContext CreateDbContext(string[] args)
        {

            var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>();
            optionsBuilder.UseSqlServer(configuration.GetConnectionString("DefaultConnection"));

            return new AppDbContext(optionsBuilder.Options);
        }
    }
}
```

appsettings.json
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=LEGION\\SQLEXPRESS;Database=DataBaseFirstDemo;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

in main
```c#
using (var scope = app.Services.CreateScope())
{
    var dbContext = scope.ServiceProvider.GetRequiredService<ApplicationDbContext>();
    // Now you can use dbContext
    // Example: dbContext.Database.EnsureCreated();
    // Or: await dbContext.Database.MigrateAsync();
}
// or 
internal class Program
{
	static void Main(string[] args)
	{
		Console.WriteLine("Hello, World!");
		
		var configuration = new ConfigurationBuilder()
		 .SetBasePath(@"D:\Projects\Starter\Starter\") // project root
		 .AddJsonFile("appsettings.json")
		 .Build();
		
		
		var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>();
		optionsBuilder.UseSqlServer(configuration.GetConnectionString("DefaultConnection"));
		
		using var context = new AppDbContext(optionsBuilder.Options);
		context.Database.EnsureCreated();
        context.Database.Migrate();
		Console.WriteLine("Connected successfully!");
	}
}

```

```c#
public class AppDbContext : DbContext
{
	public DbSet<Post> Posts { get; set; }

	public AppDbContext(DbContextOptions<AppDbContext> options)
		: base(options)
	{
	}
}
```