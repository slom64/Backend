- We use it to get reduce of null checks.
- If you have things like customer with type: Gold, Silver, Bronze. And there is null. Instead of check for null, Create strategy behaviour based on null and you can make do nothing. so you will do what you have did for Gold, Silver... but create new one for null that do nothing special.

```java
public class NullDiscountStrategy implements Discount{
	public double calculateDiscount(double totalPrice)
		return 0;
}
```