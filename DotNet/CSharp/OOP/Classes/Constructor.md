- There is special thing in `C#` is that you can call other constructors before executing the current custom constructor by using the `:this()`
	- Having mulitple calls of other constructors is bad practice, you can use it only to call the default constructor to create the needed objects otherwise don't call other custom constructors 

```c#
namespace CSharpApplication
{
	public class Customer
	{
		private int Id;
		private string Name;
		private List<Order> orderList;
		
		public Customer()
		{
			orderList = new List<Order>();
		} 
		
		public Customer(int id) 
			:this()
		{
			this.Id = id;
		}
		
		public Customer(int id,int name)
			:this(id)
		{
			this.Name = name;
		}
	}
}

// Note if we didn't use :this() then we used the Customer(id) constructor in our code, the orderList object won't be created and it will pointing at null
```

- In `c#` there is special way to intialize objects without using constructors
```c#
var person  = new Person
				{
					FirstName = "Mosh",
					LastName = "Hamadani"
				}
```