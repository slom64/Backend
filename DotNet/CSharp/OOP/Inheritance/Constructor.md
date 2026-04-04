- When we do inheritance, the parent constructor will be executed first even if you didn't write it.
- You can choose which parent constructor to be used using `:base()`
```c#
public class Car : Vehicle
{
	public Car (string registrationNumber)
		:base(registrationNumber)
	{
		// anything.
	}
}
```