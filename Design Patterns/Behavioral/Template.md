- Use it when you are sure you want to do 1,2,3 but you don't know actually how 1 or 2 or 3 are implemented.
- Example if we have shopping business offline and online. but the online method give discounts for clients

![[TemplateDesignPattern|800]]

---

```java
public abstract class Cart{
	// Some code.
	
	public void checkout(Customer customer){
		applyTaxes();
		applyDiscount();
		applyTranscation();
	}
	
	public void applyTaxes(){ return this.cost * 0.1 }
	public abstract void applyDiscount();
	public void applyTransaction(){ return true}
}
```

```java
public class OffLineCart extends Cart{
	public void applyDiscount(Cost cost){
		this.totalCost -= totalCost * 0.2;
	}
}
```

```java
public class OffLineCart extends Cart{
	public void applyDiscount(Cost cost){
		return 0;
	}
}
```