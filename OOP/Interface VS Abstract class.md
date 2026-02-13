## Summarizing
- When we talk about abstract classes we are defining characteristics of an object type; specifying _**what an object is**_.
- When we talk about an interface and define capabilities that we promise to provide, we are talking about establishing a contract about _**what the object can do.**_

---
## Abstract class
- Method abstraction is a very useful technique to define behavior patterns over the classes that are going to inherit from a given class we are defining.
- An abstract method is a class method which won’t be implemented but is expected to be implemented by the descendants of the class. In delphi, if you try to call that method you’ll get a runtime exception as there isn’t really any code there that can be called.
## Interface
- An interface is a way of defining a **contract**. When we talk about abstract classes we are defining characteristics of an object type, specifying **what an object is** but in the case of an interface we define a capability and we bond to provide that capability, we are talking about establishing a contract about **what the object can do.**

---

## 1. The Architectural Breakdown

If you are building a secure system on Linux, you can use both together to get the best of both worlds. This is called the **Skeletal Implementation** pattern.

- **The Interface (The Contract):** Defines _what_ can be done. (e.g., `Authenticatable`)
- **The Abstract Class (The Helper):** Provides the _how_ for common parts to save you from writing the same code in every child. (e.g., `AbstractLDAPAuthenticator`)
- **The Concrete Class (The Object):** The actual implementation you instantiate. (e.g., `LinuxUserAuthenticator`)

---

## 2. Key Differences in "Hidden" Logic

Since you asked about hidden parameters and memory earlier, here is how the JVM treats them differently:

### Abstract Class (Stateful)

- **Memory:** When you extend an abstract class, the child object's memory block **physically includes** the fields of the abstract parent.    
- **Constructor:** Even though you can't say `new MyAbstractClass()`, the abstract class has a constructor that **must** run to initialize its private fields.

### Interface (Stateless)

- **Memory:** An interface adds no fields to the object (only constants). It is a "Zero-calorie" addition to your class.
- **Resolution:** The JVM uses a special table called an **itables** (Interface Tables) to find interface methods, which is separate from the standard **vtable** used for class inheritance.

---

## 3. When the "Thread" Lesson Applies

You mentioned that `Thread` should be an interface. In modern Java, we use the `Runnable` interface because of the **"Single Inheritance Rule."**

> **The Rule:** You only get one "Identity" (Abstract Class), but you can have many "Skills" (Interfaces).

If you make something an **Abstract Class**, you are "using up" the only parent slot that your child classes have. If you make it an **Interface**, you leave that slot open for something else.

---

## 4. Comparison Summary for Web Security

When designing a web API or security module:

| **Use Abstract Class if...**                                      | **Use Interface if...**                                                                               |
| ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| You want to share **private/protected code** between children.    | You want to define a **public API** for other modules to use.                                         |
| You expect the classes to be **closely related** (Animal -> Dog). | You expect **unrelated classes** to do the same thing (File, Socket, and User all being `Closeable`). |
| You need to provide **non-static fields** (state).                | You want to ensure **total decoupling** for easier unit testing.                                      |

---

## Final Perspective: The "Sealed" Interface

In 2026, we now use **Sealed Interfaces** to get the security of a closed system with the flexibility of an interface. It allows you to say: "This is an interface, but _only_ these three specific classes are allowed to implement it."

This prevents an attacker from creating a fake implementation of your security interface to bypass checks.

Java

```
public sealed interface SecurityToken permits JWTToken, OAuthToken, SessionToken {
    boolean isValid();
}
```

---
# M

## Abstract Class vs. Interface: Comparison

| **Feature**        | **Abstract Class**                                    | **Interface**                                                  |
| ------------------ | ----------------------------------------------------- | -------------------------------------------------------------- |
| **Inheritance**    | A class can extend only **one** class.                | A class can implement **multiple** interfaces.                 |
| **State (Fields)** | Can have instance variables (state).                  | Can only have constants (`public static final`).               |
| **Constructor**    | Has constructors (called via `super()`).              | Cannot have constructors.                                      |
| **Hidden `this`**  | Receives the same `this` reference as normal classes. | Receives `this` reference for its methods.                     |
| **Relationship**   | "Is-A" (e.g., a `Dog` **is an** `Animal`).            | "Can-Do" (e.g., a `Dog` **can be** `Serializable`).            |
| **Method Types**   | Can have private, protected, and final methods.       | Mostly public abstract (until Java 8/9 added default/private). |

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
