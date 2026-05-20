- [ ] Counting Sort, O(N+K) (where K is the range of numbers)
- [ ] Radix Sort, O(N⋅w) (where w is the number of bits in the integer).


Problem
- [ ] https://www.geeksforgeeks.org/dsa/length-of-longest-subarray-in-which-elements-greater-than-k-are-more-than-elements-not-greater-than-k/
- [ ] https://www.geeksforgeeks.org/dsa/find-subarray-with-given-sum-in-array-of-integers/
- [ ] https://www.geeksforgeeks.org/dsa/given-a-sorted-and-rotated-array-find-if-there-is-a-pair-with-a-given-sum/

---
## Project

- [ ] Make exception handling more faster by override the `StackTrace` property inside your custom exception. By default, .NET walks the runtime stack to build the string when an exception is thrown. If you force the property to return `null` or your custom string directly, **the .NET runtime skips the heavy stack-walking process entirely.**
```csharp
public class DomainRuleException : Exception
{
    private readonly string? _customStackTrace;

    public DomainRuleException(string message) : base(message)
    {
        // Capture an ultra-short trace manually or leave it as null/empty
        _customStackTrace = $"[Domain Rule Failure] at {DateTime.UtcNow}";
    }

    // OVERRIDE THIS: This stops .NET from walking the stack when thrown!
    public override string? StackTrace => _customStackTrace;
} 
```

- Add `isDeleted` column in SizeVariant and ColorVaraint.