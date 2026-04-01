- Every authentication provider needs to look at dataStore to check creds and get User object. The part of retrieving data is managed by `UserDetailsService` which contain `loadUserByName()` method which takes username and return `UserDetails` Object which contain user details, which can be used in Authentication object as principal



---
### Configure
- After having `UserDetailsService` and `PasswordEncoding`
```java
@Bean  
public AuthenticationProvider authenticationProvider(){  
	var provider = new DaoAuthenticationProvider();  
	provider.setPasswordEncoder(passwordEncoder());  
	provider.setUserDetailsService(userService);  
	return provider;  
}
```