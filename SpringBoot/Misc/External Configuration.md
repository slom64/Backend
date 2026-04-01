- You can put here application configuration.
- Spring also supports `YAML` for configurations, `/resources/application.yaml`
	- It solve a problem in `application.properties` which is duplication of keys (like `stripe` in the example below). 
	- In yaml you can represent anything before the dot `.` --> `property:`. look at the example below
	- This one is more structure and readable. 
	- If there is `application.properties` file and `application.yaml`. yaml file has more priority.

```java
spring.application.name=spring
server.port=8080 // built-in
app.page-size=10                    // Integer value
stripe.enabled=true                 // boolean value
stripe.apiUrl=https://api.strip.com // String value
stripe.supported-currencies=USD,EUR,GBP
```
### YAML
```yaml
spring:
	application:
		name: spring
stripe:
	enabled: true
	apiUrl: https://api.stripe.com
	supported-currencies: USD,EUR,GBP
```

---
## Usage
```java
@Value("${stripe-timeout:3000}") // Assign default value in case the configuration aren't present in application.properties
private int stripeTimeout;

@Value("${stripe.apiUrl}")
private String stripeUrl; // spring will inject the value inside this varialbe.

@Value("${stripe.enabled}")
private boolean stripeEnabled;

@Value("${stripe.supported-currencies}")
List<String> supportedCurrencies;
```
