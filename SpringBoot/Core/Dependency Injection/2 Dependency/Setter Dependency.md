- Use this when you have optional dependency.

```java
public class OrderService {  
    PaymentService paymentService;  
    public OrderService() {  
          
    } 
    
    public void placeOrder(){  
	    paymentService.processPayment(10);  
	}
	
	public void setPaymentService(PaymentService paymentService){
		this.paymentService = paymentService;
	}
}
```