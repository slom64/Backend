- It happens because of **lazy loading**. 
	- Firstly you load the object without its relations
	- Then everytime you try to access one of its relations you need to do another query 
		- Query per element "for each object that try to access new relation object it will do another query". Exampl, if we have Course that have multiple authors, then for each author in each course we will do new query to the database.
	- And if you try to access N elements you will end up doing N+1 or N + Number of relations you try to acess.


> [!Danger]
> if we have Course that have multiple authors, Then for each author in each course we will do new query to the database. THIS IS THE PROBLEM. we will do
> `(course1,author1)` -> `(course1,author2)` -> `(course1,author3)` each one of those in separate query.


```csharp
var courses = AppDbContext.Courses;

foreach(var course in courses)
	Console.WriteLine(course.Title + " Writer is: " + course.Author.Name); // new query for each author in each course.
```