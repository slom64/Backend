- When load any object, it get tracked using DpContext, and marked as unchanged. If we did any modifications it will be marked as changed. then we should use `SaveChanges()`
- Even if you did multiple updates, it sill do one round trip to database. 
```csharp
var course = AppDbContext.Courses.Find(1);
course.Title = "asdf";
course.AuthorId = 2;

AppDbContext.SaveChanges();
```

- When you loop through your objects and change their properties, EF Core marks them as `Modified` in its internal tracker. When you hit save, the SQL sent to your MariaDB server looks like this in one go:
```sql
-- One single packet sent to the DB
UPDATE ProductImages SET DisplayOrder = 0 WHERE Id = 'guid-1';
UPDATE ProductImages SET DisplayOrder = 1 WHERE Id = 'guid-2';
UPDATE ProductImages SET DisplayOrder = 2 WHERE Id = 'guid-3';
-- ... and so on
```


> [!Attention]
> This method generate multiple sql queries but it use only one round trip to database.


---
### ExecuteUpadateAsync()
- This one may cause `N+1` problem, `ExecuteUpdateAsync` its used to bypass the EF core and execute direct database query. 
	- if you use this method in loop, it will do sql query for each loop iteration. each execution is single database round trip.
```csharp
await _context.ProductImages
    .Where(p => p.DisplayOrder > image.DisplayOrder && p.ProductId == image.ProductId)
    .ExecuteUpdateAsync(setters => setters
        .SetProperty(p => p.DisplayOrder, p => p.DisplayOrder - 1)
    );
```