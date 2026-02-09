- We want to add parameter that tells java how many times it should execute specific method.
- You can't use any datatype for parameters, the allowed datatypes **primitive**, **String** or array of any type of previous.
- You can set default value.

```java
@Target(ElementType.METHOD)  // Annotation works only for methods.
@Retention(RetentionPolicy.RUNTIME)
public @interface RunImmediately {
	int times() default 1; // This acts as variable but it is in method form. Then setting default value
}
```

```java
// Loop through all methods in class to see which one is annotated.
Cat myCat = new Cat("bob");
for(Method method : mycat.getClass.getDeclaredMethods()){
	if (method.isAnnotationPresent(RunImmediately.class)){
		
		RunImmediately annotation = method.getAnnotation(RunImmediately.class); // Getting the class annotation to be able to read times field.
		for(int i = 0 ; i < annotation.times() ; i++){
			method.invoke(myCat);	
		}	
	}
}
```