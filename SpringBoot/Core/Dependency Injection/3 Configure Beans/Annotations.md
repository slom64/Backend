- To tell **IoC container** to treat a specific class as managed bean, we can use annotations
	- `@Component`: General purpose annotation
	- `@Service`: Used for classes that contains business logic. This is an alias for `@Component`
	- `@Repository`: Classes that interact with database.
	- `@Controller`: Classes that can handle web requests.
- If you have more than one constructor in the class, then you should use `@Autowired` annotation.

### General Idea
```java
@Service  
public class OrderService {  
    private PaymentService paymentService;  
    public OrderService(PaymentService paymentService) {  
        this.paymentService = paymentService;  
    }  
    public void placeOrder(){  
        paymentService.processPayment(10);  
    }  
}

@Service  
public class PayPalPaymentService implements PaymentService {  
    @Override  
    public void processPayment(double amount) {  
        System.out.println("Processing using PayPal "+ amount);  
    }  
}

public static void main(String[] args) {  
    ApplicationContext applicationContext = SpringApplication.run(DemoApplication.class, args);  
    var orderService = applicationContext.getBean(OrderService.class);  
    orderService.placeOrder();  
}
```

### AutoWired
```java
public class OrderService {  
    private PaymentService paymentService;
	
	public OrderService(){} 
	  
	@Autowired    // if we didn't use this, java will use the first contructor and won't initialize paymentService which will lead to future errors. 
    public OrderService(PaymentService paymentService) {  
        this.paymentService = paymentService;  
    }  
    public void placeOrder(){  
        paymentService.processPayment(10);  
    }  
}
```

---
### Multiple Beans

- If we have interface that get implemented by mulitple of classes. Which class does spring will choose?
```java
public interface PaymentService {  
    void processPayment(double amount);  
}

@Service  
public class PayPalPaymentService implements PaymentService {  
    @Override  
    public void processPayment(double amount) {  
        System.out.println("Processing using PayPal "+ amount);  
    }  
}

@Service  
public class StripePaymentService implements PaymentService{   
    @Override  
    public void processPayment(double amount) {  
        System.out.println("processing using Stripe");  
    }  
}
```

- You can solve this problem by using one of those annotations
	- `@Primary`: This annotation define the default resource that should be used.
	- `@Qualifier("name")`: You choose which bean should be used while creating the object. And it has periority over `@Primary`.

---
### Primary
```java
@Service
@Primary  
public class PayPalPaymentService implements PaymentService {  
    @Override  
    public void processPayment(double amount) {  
        System.out.println("Processing using PayPal "+ amount);  
    }  
}
```

---
### Qualifier
```java
@Service  
public class OrderService {  
    private PaymentService paymentService;  
	  
    @Autowired  
    public OrderService(@Qualifier("paypal") PaymentService paymentService) {  
        this.paymentService = paymentService;  
    }  
	  
    public void placeOrder(){  
        paymentService.processPayment(10);  
    }  
}

@Service("paypal")  
public class PayPalPaymentService implements PaymentService {  
    @Override  
    public void processPayment(double amount) {  
        System.out.println("Processing using PayPal "+ amount);  
    }  
}
```