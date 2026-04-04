- Not used too much.
- Its the oppesite of `Abstract` which prevent us from overrideing methods from other classes. or inherite class
- We can achieve this by using `sealed` access modifier. And it can be applied to **Class** or **method**
```c#
public sealed class Circle : Shape // now no one can inherite this class
{
	public override void draw()
}
```

or for methods
```c#
public class Circle : Shape
{
	public sealed override void draw() // now we can't override this method in future classes that inherite this class.
}
```