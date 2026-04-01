- Used to load user details and you should implement as "UserService implements UserDetailsService".

---
### Define
```java
@Service  
@AllArgsConstructor  
public class UserService implements UserDetailsService {  
	
    UserRepository userRepository;  
	  
    @Override  
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {  
        var user = userRepository.findByEmail(username).orElseThrow(  
                () -> new UsernameNotFoundException("Can't find user " + username));  
        
        return new User(user.getEmail(),  // <- this is User object which implement UserDetails. 
                user.getPassword(),  
                Collections.emptyList());  
    }  
}
```

---
### Configure
- After having `UserDetailsService` and `PasswordEncoder` we can create `authenticationProvider`.
```java
@EnableWebSecurity  
@Configuration  
@AllArgsConstructor  
public class SecurityConfiguration  {  
  
    private UserService userService;  
    @Bean  
    public PasswordEncoder passwordEncoder(){  
        //return new BCryptPasswordEncoder();  
        return NoOpPasswordEncoder.getInstance();  
    }  
    
    
    @Bean  
    public AuthenticationProvider authenticationProvider(){  
        var provider = new DaoAuthenticationProvider();  
        provider.setPasswordEncoder(passwordEncoder());  
        provider.setUserDetailsService(userService);  
        return provider;  
    }
}
```