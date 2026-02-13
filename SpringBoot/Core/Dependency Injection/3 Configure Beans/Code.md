- This gives us more control over beans configurations and we can use conditions here. 
	- We can configure beans for 3rd-party code, we may not have access to code it self so we configure beans using code.
- Function should be **noun** not **verb**.
- We will use a class for configuration that contain one or methods for creating beans:
	- `@Configuration`: Tells spring this class is source for bean definition.
	- `@Bean`: Tells spring this is bean producer.
- By default, Spring goes to every manged bean "@Component, @Service" and runs its construct which create an object. Even if we didn't use this class in our main code, spring will create the object and we won't use it. This is the **eager** behaviour, you can change it by using **@Lazy**.
	- When the class is marked as **lazy** it will be created only when we want.

```java
@Configuration  
public class AppConfig {  
    @Value("${app.notification:email}")  
    private String notificationConfiguration;    
    
    @Bean  
    public NotificationService emailNotificationServiceBean(){  
        return new EmailNotificationService();  
    }  
    
    @Bean  
    public NotificationService SMSNotificationServiceBean(){  
        return new SMSNotificationService();  
    }  
    
    @Bean  
    public NotificationManager notificationManager() {  
        if(notificationConfiguration.equals("SMS"))  
            return new NotificationManager(SMSNotificationServiceBean());  
        else  
            return new NotificationManager(emailNotificationServiceBean());  
    }  
}
```

---
### Bean Scope
Are beans created once and reused, or are they created every time we need them?
Bean scope is used to determine how and when a bean is created in spring IoC  container, the types are:
- `Singleton`: The default one, only one instance of the bean is created ber container. This is useful for stateless, reusable objects.
- `Prototype`:A new instance of a bean object is created everytime it is request from IoC container. useful for objects that have states or used temporary.
- `Request`: A new instance of a bean object is created for each http request. And the bean get destoryed when we finish processing the request.
- `Session`: A new bean instance is created for each http session. So the bean exist in duration of a session and get destroyed when the session ends.

```java
@Bean  
@Scope("prototype")
public NotificationService emailNotificationServiceBean(){  
	return new EmailNotificationService();  
}  
```

