- When load any object, it get tracked using DpContext, and marked as unchanged. If we did any modifications it will be marked as changed. then we should use `SaveChanges()`
```csharp
var course = AppDbContext.Courses.Find(1);
course.Title = "asdf";
course.AuthorId = 2;

AppDbContext.SaveChanges();
```