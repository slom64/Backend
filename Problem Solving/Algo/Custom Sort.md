To use `Arrays.sort()` on a custom class in Java, you have two primary options. You can either make the class **"self-sorting"** by implementing an interface, or you can use an **external "sorting rule"** (useful if you don't want to modify the original class).

---
### 1. The `Comparable` Interface (Natural Ordering)

If your class has one obvious, "natural" way to be sorted (e.g., a `Student` sorted by `id`), you should implement the `Comparable` interface and override the `compareTo` method.

- **Method to override:** `public int compareTo(T o)`
- **Logic:** 
	- Return **negative** if `this` < `o`
    - Return **zero** if `this` == `o`
    - Return **positive** if `this` > `o`
```java
class Student implements Comparable<Student> {
    int id;
    String name;

    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public int compareTo(Student other) {
        // Sort by ID ascending
        return Integer.compare(this.id, other.id);
    }
}

// Usage:
Student[] students = { new Student(5, "A"), new Student(1, "B") };
Arrays.sort(students); // Works automatically!
```

---

### 2. The `Comparator` Interface (Custom/Multiple Orderings)

If you want to sort the same objects in different ways (e.g., sometimes by `name`, sometimes by `age`), or if you cannot modify the class source code, use a `Comparator`.
- **Method to override:** `compare(T o1, T o2)`    
- **Modern Java Approach:** Use Lambdas or Method References.

```java
// Sort by name using a Lambda
Arrays.sort(students, (s1, s2) -> s1.name.compareTo(s2.name));

// Or using Method References (Cleanest way)
Arrays.sort(students, Comparator.comparing(s -> s.name));
```

---
### 3. Common Pitfall: The Subtraction Trap

Avoid doing `return this.id - other.id;`. While common, this can **overflow** if the IDs are large or contain negative numbers (e.g., `Integer.MIN_VALUE - 1`).
- **The Safe Way:** Always use `Integer.compare(a, b)` or `Long.compare(a, b)`.
