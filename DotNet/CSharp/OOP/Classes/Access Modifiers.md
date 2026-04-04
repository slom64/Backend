
| Modifier             | Same Class | Same Assembly | Derived Class (Same Assembly) | Derived Class (Different Assembly) | Other Code (Different Assembly) | Description                                                                                     |
| -------------------- | ---------- | ------------- | ----------------------------- | ---------------------------------- | ------------------------------- | ----------------------------------------------------------------------------------------------- |
| `public`             | ✅          | ✅             | ✅                             | ✅                                  | ✅                               | No restriction. Accessible everywhere.                                                          |
| `private`            | ✅          | ❌             | ❌                             | ❌                                  | ❌                               | Only inside the same class/struct.                                                              |
| `protected`          | ✅          | ❌             | ✅                             | ✅                                  | ❌                               | Accessible in derived classes only.                                                             |
| `internal`           | ✅          | ✅             | ✅                             | ❌                                  | ❌                               | Accessible within the same assembly (project).<br>Used with classes. inside the same namespace. |
| `protected internal` | ✅          | ✅             | ✅                             | ✅                                  | ❌                               | Accessible if **either** same assembly OR derived class.<br>Not Used.                           |
| `private protected`  | ✅          | ✅             | ✅                             | ❌                                  | ❌                               | Accessible only in **derived classes within same assembly**.                                    |

---

## Important Nuances 

### 1. `internal` = Assembly Boundary

Think of **assembly = compiled project (.dll or .exe)**
- If two classes are in the same project → `internal` works
- If different projects → ❌ not accessible
### 2. `protected internal` is **OR**, not AND
This is a common misunderstanding.
Accessible if:
- Same assembly ✅  
    **OR**
- Derived class (even in another assembly) ✅
### 3. `private protected` is **AND**
Much stricter:
Accessible only if:
- Derived class ✅  
    **AND**
- Same assembly ✅
### 4. Default Access Levels (important in interviews)

| Context           | Default Modifier |
| ----------------- | ---------------- |
| Class (top-level) | `internal`       |
| Class members     | `private`        |

## Example

```csharp
public class BaseClass
{
    private int a = 1;
    protected int b = 2;
    internal int c = 3;
    protected internal int d = 4;
    private protected int e = 5;
}
```

```csharp
public class Derived : BaseClass
{
    void Test()
    {
        // a ❌ not accessible
        Console.WriteLine(b); // ✅
        Console.WriteLine(c); // ✅ (same assembly)
        Console.WriteLine(d); // ✅
        Console.WriteLine(e); // ✅ (only if same assembly)
    }
}
```

---

## Mental Model (Quick Recall)

- `private` → only me
- `protected` → me + children
- `internal` → my project
- `protected internal` → children OR project
- `private protected` → children AND project
- `public` → everyone