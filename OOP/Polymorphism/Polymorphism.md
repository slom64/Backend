- When class inherte other class methods, the child can do `function overloading`, 	
### Function overloading
- Overload the same function name with different implementations.
- Can happen inside the class itself
- There is inhertance between the classes.
- Function name in child is same as function name in parent.
- Different parameter list
	- Data type
	- Num of parameters
	- Arrange of parameters

> [!NOTE]
> `return type` doesn't count in overloading.


```java
public void draw(String s){}
public void draw(int s){}
public void draw(String s, int b){}
```

### Function overriding
- Override the function impelmentation.
- Happens in different classes that have inhertance between them.
- The parameters should have the same signature: datatype, arrange.

```java
public class parent{
	public int play(int x){return x * 5};
}

public class child extends parent{
	public int play(int x){return x * 5555};
}
```