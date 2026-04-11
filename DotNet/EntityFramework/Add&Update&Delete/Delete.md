- You need to be careful for cascade delete between entities, You can configure it using Entity Configurations.
- You should think twice before deleting objects, and sometimes its better to put logical delete instead of actual delete as having `IsDeleted` Flag.

---
### Cascade Delete
```csharp
var course = AppDbContext.Courses.Find(1);
AppDbContext.Courses.Remove(course);
AppDbContext.SaveChanges();
```

---
### Cascade Delete Off
- Deleting one side of the relation my cause issues. Example, If we have relation 1 to many between author and course. then if we deleted specific author only, that will raise exception because EF don't know what to value should it put in courses to represent the AuthorId. So you have two options:
	- Put a default value in the FK of deleted objects. So but the AuthorId in courses as other value.
	- Remove all related entites that connected to this object. "Manual Cascade Delete".

```csharp
// This will raise exception.
var author = AppDbContext.Authors.FirstOrDefault();
AppDbContext.Authors.Remove(author);
AppDbContext.SaveChanges();
```

