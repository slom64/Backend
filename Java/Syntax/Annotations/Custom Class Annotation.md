### Creating
- Createing new annotation is similar of creating new class, instead of **class** keyword we will use `@interface`.
- In order to define where to use the annotation "before Class, Method, variable", we will use another annotatino called **@Target**. if you didn't put it, it will be used before anything.
	- `ElementType.TYPE`: For classes
	- `Element.METHOD`: For methods
- To configure when to use or remove the annotaion we will use `@Retention`
	- `@Retention(RetentionPolicy.RUNTIME)`: the annotation will stay throgh compile and runtime, so other classes can access this in runtime.
	- `@RetentionPolicy.CLASS`: The annotation will present through compilation but will be removed in runtime.
	- `@RetentionPolicy.SOURCE`: java will remove this annotation even before compile code, EX. Remove warnings like `@SuppressWarnings`
 

```java
@Target({ElementType.TYPE, ElementType.METHOD})  // Annotation works only for class, methods.
@Retention(RetentionPolicy.RUNTIME)
public @interface VeryImportant {  
}
```

---
### Usage
- We will use `class.getClass().isAnnotatedPresent(VeryImportant.class)` to check if a class have been annotated or not.
	- This is used against classes **not objects**.

```java
Cat myCat = new Cat("bob");
if(myCat.getClass().isAnnotationPresent(VeryImportant.class))
	System.out.println("This class is very important");
```
