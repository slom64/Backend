- Eager loading solves **N+1** problem of lazy loading. 
- Be careful when using Eager loading, if you have **ToString** or **Serialization** methods, in both objects that may make `infinite loop`

> [!danger]
> Be carful when using `ToString()` or `Serialization` methods. Because they may make `infinite loop` when using **eager loading**.

---
### How to enable it
- Using `Select()`
	- **The "Select All" Problem:** `.Include()` fetches every column from the joined tables. If your `Author` table has 20 columns but you only need the `Name`, `.Include()` wastes bandwidth and memory.
	- **Reduced Data Transfer:** By projecting only the fields you need into a `CourseDto`, you minimize the payload between the database and your application.
	- **Automatic Joins:** You don't have to manually call `.Include(c => c.Author)`. EF sees the path `c.Author.Name` and "implicitly" joins the tables.
	- **No Tracking (By Default):** Projections to non-entity types (like a DTO) are not tracked by the `DbContext`. This results in a performance boost for read-only operations because EF doesn't have to set up the change-tracker machinery.
```csharp
// Join and projection.
var courses = AppDbContext.Courses
    .Select(c => new CourseDto
    {
        Title = c.Title,
        Price = c.FullPrice,
        AuthorName = c.Author.Name,
        TagNames = c.Tags.Select(t => t.Name).ToList()
    })
    .ToList();
```

- Using `include()`, it join the current table with the target table, The problem is it **include all columns**.
```csharp
var courses = AppDbContext.Courses.Include(c => c.Author);

// you can include more
var courses = AppDbContext.Courses.Include(c => c.Author)
				.ThenInclude(c => c.Address); // This one is from Author prospective
				
```
