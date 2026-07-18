```csharp
public class Employee : IEquatable<Employee>
{
    public int Id { get; }
    public string Name { get; }

    public Employee(int id, string name)
    {
        Id = id;
        Name = name;
    }

    // 1. Implement IEquatable<T>.Equals for strongly-typed, high-performance comparison
    public bool Equals(Employee? other)
    {
        if (other is null) return false;
        if (ReferenceEquals(this, other)) return true;
        
        return Id == other.Id && Name == other.Name;
    }

    // 2. Override the standard Object.Equals to protect against weakly-typed lookups
    public override bool Equals(object? obj)
    {
        return Equals(obj as Employee);
    }

    // 3. Override GetHashCode using HashCode.Combine
    public override int GetHashCode()
    {
        // HashCode.Combine automatically handles mixing algorithms and prevents collisions
        return HashCode.Combine(Id, Name);
    }
}
```

## 3 Critical Rules for .NET Hashing
1. **Keep properties Immutable:** Notice that the properties in the example use `{ get; }` without a `set`. If you change an object's property while it is inside a `Dictionary`, its hash code changes. The dictionary will look for it in the wrong bucket, rendering the item permanently lost inside memory.
2. **Synchronize Equals and GetHashCode:** If `Equals` says two objects are the same, `GetHashCode` **must** return the exact same integer for both. If they don't match, your `Dictionary` will result in duplicate keys or fail to find your data.
3. **Handle Nulls Safely:** When checking strings or other reference types inside your methods, ensure you use safe operators (like `HashCode.Combine` which handles null values automatically) to avoid throwing a `NullReferenceException`.