- If you want to achieve synchronization in your code, where the thread doesn't stop in blocking operation like disk reading, download from web. Then you should use `Async/Await`
	- You can use await keyword only inside async function
- For each Async function they should return Task.
	- If your function return void then use `Task`
	- if your function return any object the use `Task<>`
- When we flag a function as await, the complier knows that this function will take time, so it save a callback to this function. And the thread complete execution as if it has returned from context that it was running from. When the await function finish, the thread complete execution from from that await.
- Most functions that need synchronization have another implementation that work with sync that return Task. Ex. `webClient.DownloadStringTaskAsync()`, `streamWriter.WriteAsync()`

```c#
public async Task DownloadHtmlAsync(String url)
{
	var webClient = new WebClient();
	var html = await webClient.DownloadStringTaskAsync(url);
	
	using ( var streamWriter = new StreamWriter(@"c:\projects\result.html"))
	{
		await streamWriter.WriteAsync(html);
	}
}

private void Button_Click()
{
	DownloadHtmlAsync("http://asdf.com");
}
```

```c#
public async Task<string> GetHtmlAsync(string url)
{
	var webClient = new WebClient();
	
	return await webClient.DownloadStringTaskAsync(url);
}

private async void Button_Click()
{
	var getHtmlTask = GetHtmlAsync("http://asdf.com");
	MessageBox.Show("Waiting for the task to complete"); // you can do work after the blocking operation, and it won't hult.
	
	var html = await getHtmlTask; // you can delay your await, instead of direct await on blocking method
	messageBox.Show(html)
}
```