- Cookies must be added to the response before writing any other response elements.
- A Cookie can either be stored on session level or on persistence level
	- A **session** level cookie is a cookie that is stored in the browser's memory and deleted when the user quits the browser
	- A **persistent** cookie is stored on the client's hard disk. To make your cookie persistent, use the method `setMaxAge(int seconds)` before adding it to the response.

### Add Cookie to the response
```java
Cookie c1 = new Cookie("myCookieName", "myValue");
response.addCookie(c1);
```

### Retrieve cookie from request
```java
Cookie[] cookies = req.getCookies();

if(cookies != null)
{
	for(int i = 0 ; i < cookies.length; i++)
	{
		Cookie cookie = cookies[i];
		out.println("cookie Name: " + cookie.getName() + "cookie Value: " + cookie.getValue());
	}
}
```