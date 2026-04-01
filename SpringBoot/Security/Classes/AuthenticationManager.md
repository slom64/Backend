- It has a method called `authenticate()`, we use `authenticationManagerBuilder` to configure it. we give the generated authenticationManger to our security configuration.
- HttpSecurity for authorization
- `AuthenticationProvider` takes `Authentication` Object as input that has the `cerdentials` and give back out `Authentication` Object with `principle`.
- AuthenticationManager supports mulitple of `AuthenticationProviders`, in runtime it asks every provider if it support specific method of authentication "Ex. LDAP". Every AuthenticaionProvider has `supports()` method to know what auth mechanism does it supports.

---

> [!Question]
> After defining UserDetailsService and PasswordEncoder we configure the AuthenticationProvider, cool but when we configure the authenticationManager i don't see it taking the the AuthenticationProvider that we have created.

#### 1. The "Automatic Wiring" Secret

When you call `authenticationConfiguration.getAuthenticationManager()`, Spring doesn't just create a blank manager. It performs a **Lookup**:
1. It looks into the **Application Context** (the big bucket of Beans you've defined).
2. It finds any Bean that implements the `AuthenticationProvider` interface.
3. Since you defined `public AuthenticationProvider authenticationProvider()`, Spring finds it and **automatically injects it** into the `AuthenticationManager` it is building.
#### 2. Can you have more than one?
Yes! This is why the `AuthenticationManager` is designed this way. You could define three different `AuthenticationProvider` beans:
- One for Database (`DaoAuthenticationProvider`)
- One for LDAP
- One for a 3rd party OIDC

The `AuthenticationManager` will collect **all of them**. When you try to authenticate, it iterates through them until one of them says, "I recognize this user/password."

---
### Configure
- After having `UserDetailsService` and `PasswordEncoder` then creating from them `authenticationProvider`, then we register a bean as default `authenticationManager` for spring 
```java
@Bean  
public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {  
    return authenticationConfiguration.getAuthenticationManager();  
}
```

---
### Usage
- Inside "login" Controller call our `authenticationManager`
```java
private final AuthenticationManager authenticationManager;

@PostMapping("/login")  
ResponseEntity<Void> login(RequestEntity<LoginRequest> request){
	try{
		authenticationManager.authenticate( // if this throws expection, that mean the authentication has faild
									new UsernamePasswordAuthenticationToken(
										loginRequest.email(), 
										loginRequest.password()
										));
	}catch(Expection e){}
return ResponseEntity.ok().build();
}
// or without using try-catch

@ExceptionHandler(BadCredentialsException.class)  
public ResponseEntity<Void> handBadCredentialsException(){  
    return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build();  
}
```