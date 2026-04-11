- This is good for desktop applications, but bad for web applications. because it do additional SQL queries that can be done in one query.
- Its the main reason for **N+1** problem.
### How to enable it
- Use `virtual` keyword. 
- Behind the scene, c# create another class called proxy Ex.**CourseProxy**, this class firstly will accept having null objects of fields that flaged as `virtual` but after that when we try to access it, the class will automatically load the data. 
```csharp
public class Course
{
	public int Id { set; get; }
	public string Title { set; get; }
	public virtual IList<Tag> Tags { get; set; }
}
```
#### How to Disable it
- If you didn't put `virtual`, the field we be eager loading.
- You can use configurations to guarantee that the application won't use it.
---
### How it works
```csharp
// First EF will load Course entity alone, it won't load any other entities
var course = AppDbContext.Courses.Single(c => c.Id == 2);

// This will force EF to do another SQL query to get the tags.
foreach(var tag in course.Tags)
	Console.WriteLine(tag.Name);
```


> [!Important]
> If you inspect objects that have lazy loading objects in debugger, you won't see null. Because when you try to inspect the object, it will automatically load them for you. So you won't see null object.
