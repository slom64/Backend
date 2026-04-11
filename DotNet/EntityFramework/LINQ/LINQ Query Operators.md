```c#
var cheaperBooks = 
	from b in books
	where b.price < 20
	orderby b.title
	select b.title;
```