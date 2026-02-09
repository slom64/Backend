- This is used for making code less reduntant. if you have multiple attributes and methods are common between classes, you can use `abstract` class that abstract those attributes and methods, which other classes will use them.
- You don't define the methods return type.
- Abstract class can has normal, abstract methods. The child classes that extends this abstract class should implement all abstract methods. 
  
```java
public abstract class Animal {  
    int numberOfLegs;  
    int speed;  
    int name;  
    abstract public void makeSound();    
    public void run()  
    {  
        System.out.println("Running");  
    }  
}

public class Dog extends Animal{  
    @Override  
    public void makeSound() {  
        System.out.println("wof wof!");  
    }  
}

```