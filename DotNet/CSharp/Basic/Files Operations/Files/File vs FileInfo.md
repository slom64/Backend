- **File** provides static methods, but **FileInfo** provides instance methods "Object".
- If you have small number of operations then use **File** because operating system do some security checks on authorization to access the file and those checks happen for each method call, but if you used **FileInfo** it will check once

---
```csharp
public class FileService(IWebHostEnvironment _env) : IFileService
{
	public string DeleteImageAsync(string filePath)
	{
		var absPath = Path.Combine(_env.WebRootPath, filePath.TrimStart('/'));
		if (File.Exists(absPath))
		{ 
			File.Delete(absPath);
			return "Image is deleted";
		}
		return "Image doesn't exists";
	}
	
	public async Task<string> SaveAndResizeVariantImageAsync(IFormFile file,string imageName,String directory = null)
	{
	
		var folderPath = Path.Combine(_env.WebRootPath, "images");
		if(directory != null)
			folderPath = Path.Combine(_env.WebRootPath,directory);
		var fileName = $"{imageName}.webp";
		var filePath = Path.Combine(folderPath, fileName);
		
		using var image = await Image.LoadAsync(file.OpenReadStream());
		
		// Resize to a square 800x800, cropping the excess to maintain aspect ratio
		image.Mutate(x => x.Resize(new ResizeOptions
		{
			Size = new Size(800, 800),
			Mode = ResizeMode.Crop
		}));
		
		await image.SaveAsWebpAsync(filePath); // High compression, high quality
		return Path.GetRelativePath(_env.WebRootPath, filePath);
	}
}
```