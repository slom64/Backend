- Use this to have better communction between services and controllers.
```csharp
public class ServiceResponse<T>
{
	public bool Succeeded { get; set; }
	public T? Data { get; set; }
	public string? Error { get; set; }


	public static ServiceResponse<T> Success(T data) =>
		new() { Succeeded = true, Data = data };

	public static ServiceResponse<T> Failure(string error) =>
		new() { Succeeded = false, Error = error };
}

```