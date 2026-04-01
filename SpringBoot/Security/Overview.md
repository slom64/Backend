- `Athentication Filter` intercept the request and create **authentication** object with creds, and passes it to `AuthenticationManager`.
- `AuthenticationManager` choose the right `AuthenticationProvider`
- `AuthenticationProvider` takes the `Authentication` object and uses UserDetailsService to get user details, then the authenticationProvider verify the details, then the authentication happens and return Authentication object with UserDetails/principal and authorities
- If the authentication fails, the authenticationProvider throws exception and the filter catch this exception.
- If all is good the filter saves the authentication object in security context to use it later for authorization and other things.


---
## Steps For developing
- Start by creating your `UserDetailsService`, which then you may use `User` class or implement your own `UserDetails`.
- Create your `passwordEncoder`
- Now you have `DaoAuthenticationProvider` elements, you can configure it as a bean in our securityConfiguration as authenticationProvider.
- Now you should set the authenticationManager.
- Now we should set AuthController to use our authenticationManager

https://www.youtube.com/watch?v=caCJAJC41Rk&list=PLqq-6Pq4lTTYTEooakHchTGglSvkZAjnE&index=6
![[Z Assets/Images/Pasted image 20260224225057.jpeg]]

```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-web</artifactId>  
</dependency>  
  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-security</artifactId>  
</dependency>
```