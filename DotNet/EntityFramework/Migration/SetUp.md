```c#
dotnet add package Microsoft.EntityFrameworkCore.Design

dotnet ef migrations add InitialCreate // This generates a **C# migration file** inside a `Migrations` folder.
dotnet ef database update // Creates tables in the database automatically ,No App.config required, uses `appsettings.json` connection string
```



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
        public AppDbContext CreateDbContext(string[] args)
        {
            // Build configuration to read appsettings.json
            var configuration = new ConfigurationBuilder()
                .SetBasePath(@"D:\Projects\Starter\Starter\")
                .AddJsonFile("appsettings.json", optional: false)
                .Build();

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