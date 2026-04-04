- To make a method overridable you should but `virtual` before its signature.
```java
public class Shape
{
	public virtual void draw(){};
}
```

- To override `virtual` method we use `override` before the method signature.
```c#
public class Circle : Shape
{
	public override void draw()
}
```