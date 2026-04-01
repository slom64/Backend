```java
public enum Role {  
    USER,  
    ADMIN  
}

// inside the User class add this
@Column(name = "role")  
@Enumerated(EnumType.STRING)  // Tells spring to store the value as string
private Role role;



```