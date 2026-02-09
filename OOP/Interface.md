- If the child that implements interface didn't define all the abstract methods, then this child class will be abstract class.
- You can't create objects from interface
## Abstract Class vs. Interface: Comparison

|**Feature**|**Abstract Class**|**Interface**|
|---|---|---|
|**Inheritance**|A class can extend only **one** class.|A class can implement **multiple** interfaces.|
|**State (Fields)**|Can have instance variables (state).|Can only have constants (`public static final`).|
|**Constructor**|Has constructors (called via `super()`).|Cannot have constructors.|
|**Hidden `this`**|Receives the same `this` reference as normal classes.|Receives `this` reference for its methods.|
|**Relationship**|"Is-A" (e.g., a `Dog` **is an** `Animal`).|"Can-Do" (e.g., a `Dog` **can be** `Serializable`).|
|**Method Types**|Can have private, protected, and final methods.|Mostly public abstract (until Java 8/9 added default/private).|

---

## When to Use an Interface

In a modern, secure Java application (especially in a Linux web environment), you use an **Interface** when you want to define **behavior** without caring about the **identity** of the object.

### 1. Decoupling (The "Plug-and-Play" Pattern)

If you are writing a service that sends notifications, you don't want to hard-code it to "Email."
```java
public interface Notifier {
    void send(String message);
}
```

Now, you can have `EmailNotifier`, `SmsNotifier`, or `SlackNotifier`. Your main code only knows about the `Notifier` interface. This makes your code much easier to test and maintain on a server.

### 2. Multiple Inheritance of Behavior

Since Java doesn't allow a class to have two parents (to avoid the "Diamond Problem" in memory), interfaces are the only way to give a class multiple "roles."
- A `UserAccount` class might extend `BaseDatabaseEntity` (Abstract Class)
- **AND** implement `Authenticatable` (Interface)
- **AND** implement `Authorizable` (Interface)    
### 3. API Security & Design

When you expose a library to others, you often return an Interface. This hides the internal "Child" class and its private fields from the user. They only see the methods you explicitly exposed in the interface.

---

## The "Hidden" JVM Perspective

Remember our discussion about the **vtable** (Virtual Method Table)?

- **Abstract Classes** use **`invokevirtual`**: This is very fast. The JVM knows exactly where the method lives in the parent/child hierarchy.
- **Interfaces** use **`invokeinterface`**: This is slightly more complex for the JVM. Because a class can implement many interfaces, the JVM has to do a bit more work to find the correct method in memory.

---

## Summary

- **Abstract Class:** Use it for **Internal logic**. It’s where you put shared code (like a base controller that handles logging for all your web endpoints).
- **Interface:** Use it for **External communication**. It defines the "Contract" that different parts of your system use to talk to each other without knowing the underlying memory details.

**A Security Tip:** Always use the most restrictive interface possible. If a method only needs to "Read" data, don't pass it the full `UserObject`; pass it a `ReadOnlyUser` interface. This is a classic "Principle of Least Privilege" implementation in Java code.
