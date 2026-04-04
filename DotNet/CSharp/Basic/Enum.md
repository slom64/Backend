- Enum are represented internaly as integers, but you can change that to byte
- The enum will start by defaul value 0, but its recommended to assign values to them to match the database entries.

```c#
namespace pg
{
	public enum ShippingMethod
	{
		Express = 1,
		RegularAirMail = 2,
		RegisteredAirMail = 3
	}
	
	class Program
	{
		static void Main(String []args)
		{
			var myEnum = ShippingMethod.Express;
			
			// Convert Enum -> String
			Console.Write(myEnum.ToString());
			
			// Convert Enum -> integer
			Console.Write((int)myEnum); // "3" You can easily cascade to integer.
			
			// Convert integer -> Enum
			int methodId = 3;
			Console.Write((ShippingMethod)methodId) // "Express"
			
			
			// Convert String -> Enum
			var methodName = "Express";
			var shippingMethod = (ShippingMethod) Enum.Parse(typeof(ShippingMethod), methodName);
		}
	}
}
```