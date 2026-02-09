
```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface ImportantString {
	
}
```

```java
// Loop through all fields in class to see which one is annotated.
Cat myCat = new Cat("bob");
for(Field field : myCat.getClass.getDeclaredFields()){
	if (field.isAnnotationPresent(ImportantString.class)){
		Object object = Field.get(myCat)   // get the field value of specific object.
		
		if(object instanceof String stringValue){ // Check if object is instance of String and upcaste it and put the value in stringValue
			System.out.println(stringValue.toUpperCase());
		} 
	}
}
```