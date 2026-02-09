- It's an object that's get created when an abnormal situation arises in your program during runtime. Example: open non existance file, open high privilege file.

## Throwable
- `Error`: abnormal situation that the developer can't handle, Ex: Out of memory.
- `Exception`
	- `Runtime, Unchekced Exception`: developer can't handle during runtime too. Ex: `NullPointerException`--> logic problem, Zero division
	- `Checked Exception`: developer can handle this. file operations, I/O operations.



```java
public class test{
	public void testMethod(){
		try{
			readFile(); // code we will try to execute and may arise exception
		}
		catch(XYZexception ex)
		{
			// code we will execute if exception get arised
		}
		final{
			// code will get executed even if exception has arised or not.
		}
	}
}
```

### Execption delegation
we will delegate the implementation of the exception to the caller of our method, or the caller again throws exception.
```java
public class test{
	public void testMethod() throws XYZexception{ // now who calles this method should handle the exception
		readFile();
	}
}
```
