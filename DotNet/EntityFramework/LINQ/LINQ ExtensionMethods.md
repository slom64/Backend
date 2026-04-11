### Filtering
```c#
var x = AppDbcontext.Courses
	.Where(c => c.FullPrice > 50);
```
---
### Ordering
```c#
var x = AppDbcontext.Courses
	.Where(c => c.FullPrice > 50)
	.OrderByDescending(c => c.FullPrice).ThenBy(c => c.Title) // Sort by 2 elements
```

---
### Projection
- You can have anonymous object if you don't want to create DTO.
```c#
var x = AppDbcontext.Courses
	 .Where(c => c.FullPrice > 50)
	 .OrderByDescending(c => c.FullPrice).ThenBy(c => c.Title)
	 .Select(c => c.Title);
	 // Or anonymous object
	 .Select(c => new { CourseName = c.Name, AuthorName = c.Auther.Name })
	 
	 // This will give us list of Courses that contain list of tags of each course
	 Select(c => c.Tags)
	 // This will give us pure list of tags
	 SelectMany(c => c.Tags)
```

---
### Set Operators
```c#
var x = AppDbcontext.Courses
	.Where(c => c.FullPrice > 50)
	.OrderByDescending(c => c.FullPrice).ThenBy(c => c.Title)
	.SelectMany(c => c.Tags);
	.Distinct();  // Have unique results
```

---
### Grouping
```c#
var x = AppDbcontext.Courses
	.Where(c => c.FullPrice > 50)
	.OrderByDescending(c => c.FullPrice).ThenBy(c => c.Title)
	.GroupBy(c => c.Level);
```

---
### Joining
#### InnerJoin
- Used to join two tables that are unrelated togather "There is no FK between them"
```c#
// Imaging there is no relation between course and author.
var result = context.Courses  
	.Join(  
			context.Authors,  
			course => course.AuthorId, // FK  
			author => author.Id, // PK  
			(course, author) => new  // Projection, anonymous object
			{  
				CourseName = course.Title,  
				AuthorName = author.Name  
			}  
		)  
		.ToList();
```
#### GroupJoin
- Its same as left Join by in SQL
- So we will have multiple of objects per object.
```c#
var result = context.Authors
    .GroupJoin(
        context.Courses,
        author => author.Id,        // outer key (PK)
        course => course.AuthorId,  // inner key (FK)
        (author, courses) => new
        {
            Author = author,
            Courses = courses // return the whole object "No projection".
        }
    )
    .ToList();
```

#### Cross Join
```c#
var result = context.Authors
    .SelectMany(
        a => context.Courses,
        (a, c) => new
        {
            AuthorName = a.Name,
            CourseTitle = c.Title
        }
    )
    .ToList();
```

---
### Partitioning / pagination
```c#
var x = AppDbcontext.Courses
	.Where(c => c.FullPrice > 50)
	.Skip(10).Take(10);
```

---
### Element Operators
- `Single`
	- Expects **exactly one** matching element. It throws an `InvalidOperationException` if more than one match is found or no match found.
	- use `SingleOrDefault` to stop raising exception and instead it will return default value "for primitive types it will be null".
- `first`
	- Returns the very first matching element it finds and ignores any subsequent matches
	- If there is no match it will raise exception so instead use `firstOrDefault`