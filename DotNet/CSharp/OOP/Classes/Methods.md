- Methods can have arugments that can be flaged as `params`, `ref`, `out`.
- If your method may have multiple parameters and its unknown number then you can use `params`
```c#
public class Calculator
{
	public int add(params int [] numbers); 
}

var result = Calculator.add(new int[]{1,2,3,4})
// or you can use directly
var result = Calculator.add(1,2,3,4);
```

- If you want to have mulitple return values you should use `out`. "BAD PRACITCE its better to define a class and put it as return value of the method"
```c#
public class MyClass
{
	public void MyMethod(out int result)
	{
		result = 1;
	}
}

int a ;
myClass.MyMethod(out a);
```

- If you want to pass by value an argument use the `ref` "BAD PRACTICE"
```c#
public class Weirdo
{
	public void DoWeirdThing(ref a)
	{
		a += 2
	}
	var a = 1;
	weird.DoWeirdThing(ref a); // 3	
} 
```