
```java
@Target(ElementType.METHOD)  // Annotation works only for methods.
@Retention(RetentionPolicy.RUNTIME)
public @interface RunImmediately {  
}
```

```java
// Loop through all methods in class to see which one is annotated.
Cat myCat = new Cat("bob");
for(Method method : mycat.getClass.getDeclaredMethods()){
	if (method.isAnnotationPresent(RunImmediately.class))
		method.invoke(myCat); // Run the annotated method for specific target object.
}
```