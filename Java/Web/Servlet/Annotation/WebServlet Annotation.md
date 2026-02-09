- Instead of writing configurations inside the web.xml, we can use annotations instead.
- In order to be able to use annotations, we should put the following in web.xml inside `web-app` at the end: `metadata-complete="false"`

```java
@WebServlet("proccessForm")
public class TheServlet extends javax.servlet.http.HttpServlet{}

@WebServlet(urlPatterns="sendFile", "uploadFile")
public class TheServlet extends javax.servlet.http.HttpServlet{}
```