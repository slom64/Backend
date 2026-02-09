- Here you put Servlet configurations.


```xml
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                             https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">


    <servlet>
        <servlet-name>myservlet</servlet-name> <!--Name of servlet inside tomcat --> 
        <servlet-class>com.servlet.MyServlet</servlet-class> <!--Path of servlet -->
    </servlet>
    <servlet-mapping>
        <servlet-name>myservlet</servlet-name> <!-- The servlet name that we have choose inside tomcat-->
        <url-pattern>/hello</url-pattern> <!-- URL path that we will use to access this servlet -->
    </servlet-mapping>
	<welcome-file-list> <!-- List of files we try to open by default, they behave as index.html -->
	    <welcome-file>hello</welcome-file>  
	    <welcome-file>hello2</welcome-file>  
	</welcome-file-list>    

</web-app>
```