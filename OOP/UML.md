### Inheritance relationship 
```java
public class Rectangle extends Shape{
	
}
```

![[UMLInheritanceRelationship|200]]

---
### Composition Relationship
- When we have field of another class.
```java
public class Shape{
	Private Size size;
}
```

![[UMLCompositionRelationship|200]]

---
### Aggregation Relationship


---
### Dependency Relationship
- When we use another class inside method. "It's not field in the class"
```java
public class Shape{
	public void render(Document doc){}
}
```

![[UMLDependencyRelationship|200]]