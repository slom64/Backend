- If you repeat snippet of code multiple of times across your project, then create simple class that implement this code and just call this class


---
### Before
```java
//main.java

if(cusotmer.category == Enum.Silver)
	doSomething();
else(cusotmer.category == Enum.Bronze)
	doSomething();
else(cusotmer.category == Enum.Gold)
	doSomething();

// transaction.java
if(cusotmer.category == Enum.Silver)
	doSomething();
else(cusotmer.category == Enum.Bronze)
	doSomething();
else(cusotmer.category == Enum.Gold)
	doSomething();

```

---
### After
- Create class that has method that we will call it instead of copy/paste multiple of lines
- Naming convension is 
	- End the class name with **Factory**. "CustomerDiscountStrategy**Factory**"
	- Start the method name with **Create**. "**Create**CustomerDiscountStartegy"

```java
public class CustomerDiscountStrategyFacotry{
	public void CreateCustomerDiscountStartegy(Customer customer){
		if(cusotmer.category == Enum.Silver)
			doSomething();
		else(cusotmer.category == Enum.Bronze)
			doSomething();
		else(cusotmer.category == Enum.Gold)
			doSomething();
	}
}
```
Now you can call it with single line
```java
CustomerDiscountStrategyFacotry customerDiscountStrategyFacotry = new CustomerDiscountStrategyFacotry();
CustomerDiscountStrategyFacotry.CreateCustomerDiscountStartegy(customer)
```