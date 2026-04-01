- This interface represent the general format of the user details that would be used in spring security.
- There is built in implementation of this interface which is `User`. or you can implement your own `UserDetails` by implementing this interface.


---
### Usage
- `UserDetails` used as return type of `UserDetailsService`
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