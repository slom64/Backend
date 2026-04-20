- If we have class `Course` then the framework will create table with the plural form which is `Courses`
- The Properties Names in class are the same as in tables.
- If there is field that has ID or classNameID it will be a primary key
- Strings are nullable
- Strings use maxLength(255)
- Foreign keys creates new column with `_` like `Auther_Id`. to prevent this we do create 2 properties from the same type
```c#
public class Course
{
	// if you leaved them like this, its just like creating 2 unconnected columns.
	public int AutherId{get; set;}
	public AutherId Auther{get;set;}
}
```