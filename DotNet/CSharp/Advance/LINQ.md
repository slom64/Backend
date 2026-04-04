- Stands for: Language Integrated Query
- Gives you the capability to query objects, Example of objects:
	- Objects in memory, eg collections (LINQ to objects)
	- Databases (LINQ to Entities)
	- XML (LINQ to XML)
	- ADO.NET Data Sets (LINQ to Data Sets )


> [!important]
> Predicate is func<T, out T> or function that takes an input and return output, and most cases we use lambda expression to implement this predicate

### Methods in LINQ
- `Where`
- `Single`
	- Expects **exactly one** matching element. It throws an `InvalidOperationException` if more than one match is found or no match found.
	- use `SingleOrDefault` to stop raising exception and instead it will return default value "for primitive types it will be null".
- `first`
	- Returns the very first matching element it finds and ignores any subsequent matches
	- If there is no match it will raise exception so instead use `firstOrDefault`
- `orderBy`
- `select`: choose which fields you want to have
- `skip(n),take(m)`
	- Used for pagination data so skip the first n entries and keep taking until m.
- `Max(predicate), min, sum, Avarage`
- `Count(predicate)`
```c#
// LINQ Query Operators
var cheaperBooks = 
	from b in books
	where b.price < 20
	orderby b.title
	select b.title;
	
// Same as LINQ Extension Methods
var cheaperBooks = books
					.Where(b => b.price < 20) // this is lamba expression that only include objects that pass the condition.
					.OrderBy(b => b.Title) // choose which field to order by
					.Select(b => b.Title); // choose which fields to return "Columns"
```