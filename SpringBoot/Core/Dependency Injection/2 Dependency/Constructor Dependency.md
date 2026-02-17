- Use this when the dependent object is vital and should exists for every object in the primary class.
- This is the recommended way.

```java
// Note: In those examples we didn't use IoC of spring, we just want to show how to do dependency.
public class OrderService {  
    PaymentService paymentService;  
    public OrderService(PaymentService paymentService) {  
        this.paymentService = paymentService;  
    } 
    
    public void placeOrder(){  
	    paymentService.processPayment(10);  
	}
}

public interface PaymentService {  
    void processPayment(double amount);  
}

public class PayPalPaymentService implements PaymentService {  
    @Override  
    public void processPayment(double amount) {  
        System.out.println("Processing using PayPal");  
    }  
}

public static void main(String[] args) { 
	SpringApplication.run(DemoApplication.class, args);
	var orderService = new OrderService(new PayPalPaymentService());  
	orderService.placeOrder();  
}

```