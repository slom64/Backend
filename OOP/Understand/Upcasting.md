
> [!Question]
> if i did something like Animal dog = new Dog(); and Dog class has inheritance from animal and Dog class has attributes that like dogType doesn't exists in Animal class what will happen here? compile error or will i be unable to access the dogType attribute

In this scenario, you are performing what is known as **Upcasting**. While the object in memory is a `Dog`, the "lens" you are using to look at it is the `Animal` class.

The short answer: **You will be unable to access the `dogType` attribute directly**, and attempting to do so will result in a **compile-time error**.

---

## Why this happens: The "Contract" vs. The "Reality"

In Java, the compiler determines what you can access based on the **declared type** (the reference), not the **actual object type** (the instance in memory).

### 1. The Code Breakdown
```java
Animal dog = new Dog(); 
// Reference Type: Animal
// Object Type: Dog
```

- **The Compiler's View:** It looks at the variable `dog` and sees it is of type `Animal`. It says, "I only guarantee that this object has the features defined in the `Animal` class."
- **The Reality in Memory:** The hidden parameter (`this`) we discussed earlier still points to a full `Dog` object in the heap, complete with the `dogType` field. However, the compiler "hides" it from you to ensure type safety.

### 2. The Result

If you try to run this:
```java
System.out.println(dog.dogType); // COMPILE ERROR
```

The compiler will throw an error: `cannot find symbol: variable dogType`. It doesn't care that the memory actually contains a `dogType`; it only cares that the `Animal` class doesn't define it.

---

## How to access it: Downcasting

If you know for a fact that the `Animal` is actually a `Dog`, you must "cast" the reference. This tells the compiler to change the lens it is using.
```java
// Check first to avoid ClassCastException
if (dog instanceof Dog) {
    Dog realDog = (Dog) dog; // Downcasting
    System.out.println(realDog.dogType); // Works!
}
```

---

## Security & Memory Implications (Linux Context)

Since you are working in a **Linux/Security context**, it is interesting to note how this looks at the lower level:

1. **Vtable (Virtual Method Table):** In memory, the `Dog` object has a table of methods. Even if you treat it as an `Animal`, the hidden `this` pointer still points to the `Dog` vtable. This is why **polymorphism** works—if `Animal` has a `makeSound()` method and `Dog` overrides it, calling `dog.makeSound()` will still execute the Dog's version, even if the reference is `Animal`.
2. **Memory Layout:** On the heap, the fields are laid out contiguously. The `Animal` fields usually come first, followed by the `Dog` specific fields. When you upcast, you are essentially telling the Java environment to "ignore" the memory addresses where the `Dog` fields start.
3. **Exploitation Note:** In languages like C++, "Type Confusion" (treating one type as another) is a major security vulnerability. Java's strict compiler checks on these "hidden" references are what prevent you from accidentally reading memory you aren't supposed to.

---

### Comparison Table

|**Feature**|**Animal dog = new Dog();**|**Dog dog = new Dog();**|
|---|---|---|
|**Methods in Animal**|Accessible|Accessible|
|**Methods in Dog only**|**Inaccessible** (Compile error)|Accessible|
|**Overridden Methods**|Runs Dog's version|Runs Dog's version|
|**Fields in Dog only**|**Inaccessible**|Accessible|

---

> [!Question]
> if i have wof() method that exists in Dog but not in Animal. in runtime that won't be an issue but the problem is in compile time because compiler can't see the wof() method inside Animal class

Exactly!

At **Runtime**, the JVM is perfectly capable of finding that `wof()` method because, as we discussed, the hidden `this` parameter points to a memory block that contains the `Dog` version of the method table.

The **Compiler**, however, is a different story. It behaves like a strict bureaucrat who only follows the "contract" provided by the reference type.

---

## The "Bureaucrat" Analogy

Think of the **Reference Type** (`Animal`) as a **Job Description** and the **Object** (`Dog`) as the **Employee**.
- **The Reference (`Animal p`):** The job description says this employee must be able to `eat()` and `sleep()`. It says nothing about `wof()`.
- **The Object (`new Dog()`):** This specific employee happens to have a special skill: they can `wof()`.
- **The Compiler:** If you ask the employee to `wof()`, the Compiler looks at the **Job Description** first. Since it doesn't see "wof" listed there, it shuts the operation down immediately. It doesn't care what the employee can _actually_ do; it only cares what they are _officially_ contracted to do.

---

## Why does Java do this? (The "Safe" Why)

It might seem annoying, but it is a major security and stability feature. Imagine if you had a list of 100 `Animal` objects. Some are `Dogs`, some are `Cats`, and some are `Birds`.
```java
for (Animal a : myAnimals) {
    a.wof(); // If this were allowed...
}
```

If the compiler allowed this, your program would **crash** the moment the loop hit a `Cat` or a `Bird`. By forcing a compile-time error, Java ensures that you handle the "Check and Cast" logic safely before you ever try to access that memory.

---

## Bypassing the Bureaucrat: Reflection

Since you are interested in web security and how things work "under the hood," there _is_ a way to call `wof()` at runtime without a cast, bypassing the compiler's check. This is called **Reflection**.

```java
Animal dog = new Dog();
// Use reflection to find the method by name string
Method method = dog.getClass().getMethod("wof");
method.invoke(dog); // This works without a (Dog) cast!
```

**Security Warning:** In a Linux/Web environment, Reflection is a high-risk area. Many Java "Insecure Deserialization" exploits use Reflection to call "hidden" methods (like `Runtime.exec()`) that the programmer never intended to be accessible through a parent reference.

---

## Visualizing the Wall

The compiler creates a "Visibility Wall" based on the reference type.

### Summary of the "Problem"

1. **Memory:** The `wof()` method is there.
2. **Pointer:** The `this` parameter is ready to go.
3. **Compiler:** Blocks the path because `Animal` doesn't define the "symbol" `wof`.

**Solution:** You must use the **Downcast** `((Dog) dog).wof()` to tell the compiler: "I am taking responsibility for the fact that this memory address contains a `wof()` method."
