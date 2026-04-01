- This gives us more control over beans configurations and we can use conditions here. 
	- We can configure beans for 3rd-party code, we may not have access to code it self so we configure beans using code.
- Function should be **noun** not **verb**.
- We will use a class for configuration that contain one or methods for creating beans:
	- `@Configuration`: Tells spring this class is source for bean definition.
	- `@Bean`: Tells spring this is bean producer.
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
