## Modefiers
- This answers on question: who?
- You can't find `private class`, because private is something that is restricted to be accessed from outside the class, it would be useless to implement class that can't be used.
  You can't find `static class`, but you can find it as inner static class.
  You can't find `abstract variable`.
- `protected` is used to make things public inside the subClasses "child". `default` things are public inside the same package.


| **Modifier**                | **Same Class** | **Same Package** | **Subclass (Different Package)** | **World (Different Package)** |
| --------------------------- | -------------- | ---------------- | -------------------------------- | ----------------------------- |
| **`public`**                | Yes            | Yes              | Yes                              | Yes                           |
| **`protected`**             | Yes            | Yes              | Yes                              | No                            |
| **`default`** (no modifier) | Yes            | Yes              | No                               | No                            |
| **`private`**               | Yes            | No               | No                               | No                            |

---
## Access specifier
- Those answers on question: how?

| **Specifier**   | **Effect on Memory / Reference**                                                                                |
| --------------- | --------------------------------------------------------------------------------------------------------------- |
| **`static`**    | Removes the hidden `this` parameter. Method belongs to the class, not the instance.                             |
| **`final`**     | **Class:** Cannot be inherited. <br>**Method:** Cannot be overridden. <br>**Variable:** Constant.               |
| **`abstract`**  | Cannot be instantiated. Force a "Child" to provide the implementation.                                          |
| **`volatile`**  | Used in multi-threading on Linux to ensure a variable's value is always read from main memory, not a CPU cache. |
| **`transient`** | Prevents a field from being serialized (critical for web security—hide passwords from JSON/Disk).               |
| `synchronized`  | Uses the object's **Monitor (Lock)**. helps against deadlocks and multi thread problems.                        |
| `native`        | Jumps out of the JVM into a **Shared Library (.so)**.                                                           |

---
### native

The `native` keyword tells the JVM that the implementation of the method is **not written in Java**. It is written in a language like C or C++ and compiled into a shared library (a `.so` file on your Linux system).
- **How it works:** When you call a native method, the JVM uses the **JNI (Java Native Interface)**.

---
### synchronized
The `synchronized` keyword is Java's built-in locking mechanism to prevent race conditions.

- **The Hidden Mechanism (The Monitor):** Every single object in Java has a "hidden" lock associated with it, called a **Monitor**.
- **Instance Synchronized:** When you mark an instance method as `synchronized`, the thread must "acquire the lock" of the object (the `this` reference) before it can enter the method.
- **Static Synchronized:** If the method is `static`, the thread acquires the lock of the `Class` object itself.
```java
public synchronized void updateBalance(int amount) {
    // Only one thread can be in here for a specific object at a time.
    this.balance += amount; 
}
```