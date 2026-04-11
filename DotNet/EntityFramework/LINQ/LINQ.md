- Stands for: Language Integrated Query
- Gives you the capability to query objects, Example of objects:
	- Objects in memory, eg collections (LINQ to objects)
	- Databases (LINQ to Entities)
	- XML (LINQ to XML)
	- ADO.NET Data Sets (LINQ to Data Sets )
- You can use **Extension Methods** or **Query Operators**, Exention Methods have more capabilities.
- 


> [!important]
> Predicate is func<T, out T> or function that takes an input and return output, and most cases we use lambda expression to implement this predicate


> [!Important]
> When you do `var course = AppDbContext.Courses;` that doesn't mean that sql query have been executed against the DB. Thats good if we want to do other operations on the query before sending it like `course.orderBy`. it will be executed when `iterate over query variables`, Calling `ToList(), ToArray...` Calling `First(), Last()`
