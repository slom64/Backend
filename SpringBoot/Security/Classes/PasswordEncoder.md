- You should put it in configuration and use it to hash passwords before saving them in datebase.

---
### Configure
```java
@Bean  
public PasswordEncoder passwordEncoder(){  
	return new BCryptPasswordEncoder();  
	//return NoOpPasswordEncoder.getInstance();  
} 
```

---
### Usage
```java
@Autowire
public final PasswordEncoder passwordEncoder;

user.setPassword(passwordEncoder.encode("abc"))
```