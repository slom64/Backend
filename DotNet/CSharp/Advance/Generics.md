```c#
public class GenericDictionary<TKey,TValue>
{
	public Add(TKey key, TValue value){};
}
```

Getting Max of 2 objects
### Constraints
- Assuming that a generic class has implemented a specific class or interface, we use syntax `where T : interface`
```
public T Max<T>(T a, T b) where T : ICompareble // where is used to assume that T has implemented ICompareble.
{
	return a.CompareTo(b) > 0 ? a : b;
}
```