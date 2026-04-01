- Its an interface represents username/password, authoritise.
- There is implementation of this interface called `UsernamePasswordAuthenticationToken`This implementation has 2 constructors that serve different purposes:
	- `principal + creds` use this for authentication.
	- `principal + creds + GrantedAuthority` use this for authorization. Even though we used this as authentication filter.



---
### Usage
- When you interact with `authenticationMananger` it needs authentication object, that contain username&password
```java
private final AuthenticationManager authManager;

try{
	authManager.authenticate( // if this throws expection, that mean the authentication has faild
								new UsernamePasswordAuthenticationToken(
									loginRequest.email(), 
									loginRequest.password()
									));
}
```

- When you want to know the currently logged in user.
```

```