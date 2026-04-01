### Inheritance relationship 
```java
public class Rectangle extends Shape{
	
}
```

![[UMLInheritanceRelationship|200]]

---
### Composition Relationship
- When we have field of another class.
- The classes are strongly connected. So, When destroying the parent class that mean we will destroy the subclass. child can't exists without parent.
- its "part-of" relationship
```java
public class Shape{
	Private Size size;
}
```

![[UMLCompositionRelationship|200]]

---
### Aggregation Relationship
- ==Aggregation is a weak, part-whole relationship where children exist independently of the parent (e.g., Department-Professor), while composition is a strong, exclusive relationship where children cannot exist without the parent== (e.g., House-Room).
- So if the subclass can be found without parent class, then this is aggregation, but if the subclass should have parent class then this is composition
- So destroying the parent class doesn't mean destroying the subclass.
- It has "has-a" relationship
![[UMLAggregationRelationship|200]]

---
### Dependency Relationship
- When we use another class inside method. "It's not field in the class"
```java
public class Shape{
	public void render(Document doc){}
}
```

![[UMLDependencyRelationship|200]]