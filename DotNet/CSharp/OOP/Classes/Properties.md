- Its faster way to create setters and getters
```c#
public class Person
{
	
	public DateTime Birthdate{get; set;} // c# create private field and created the setter and getter for us.
	public string Username{get; private set;} // this create private field _username that has getter but don't has setter.
	// or do this
	private DataTime _birthdate;
	
	public DateTime Birthdate
	{
		get {return _birthdate}
		set {_birthdate = value;}
	}
	
	public int Age // This work as method exposed as a property. Everytime we access it, it recompute the value.
	{
		get{ return currentDate - _birthdate;}
	}
}
```