- This indicates that a class is missing implementation.
- We should use `abstract` modifier
```java
public abstract class Shape
{
	public abstract void Draw();
}
```

- To implement `abstract` method we use `override` before the method signature.
```c#
public class Circle : Shape
{
	public override void Draw();
}
```