- Becarful when adding new object that has other related entities, because you may add new related object while adding the main object
```csharp
var course = new Course
{
	// We didn't add Id, because it auto increment
	Title = "New Course", 
	Description = "New Description",
	FullPrice = 19.44f,
	Level = CourseLevel.Beginner,
	Author = new Author { Id = 1 , Name = "slom"} // This will create new Author, But we don't want that. We want to refrence other Author.
};
AppDbContext.Add(course);
AppDbContext.SaveChanges();
```
- reference the related objects using FK.
```csharp
var course = new Course  
{  
	Title = "New Course",  
	Description = "New Description",  
	FullPrice = 19.44f,  
	Level = CourseLevel.Beginner,  
	AuthorId = 1 // ✅ reference existing author  
};  
  
context.Courses.Add(course);  
context.SaveChanges();
```