- Automatically create setters and getters.
- You should enable annoation processing


| Annotation            | Description                                                                                                                                                                                                                                                                                                                                     |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `@Getter`             | Create getter for each field                                                                                                                                                                                                                                                                                                                    |
| `@Setter`             | Create setter for each field                                                                                                                                                                                                                                                                                                                    |
| `@AllArgsConstructor` | Create constructor that has all field as arguments<br>Using this alone my sometimes won't work, because <br>hibernate and JPA expect class with default constructor<br>"No args constructor" so append this annotation <br>with `@NoArgsConstructor`                                                                                            |
| `@NoArgsConstructor`  | Create constructor with no arguments                                                                                                                                                                                                                                                                                                            |
| `@ToString`           | Create method to convert object to String.<br>This may do `Infinite Loop` if there exists 2 connected classes and both have `@ToString`. To solve this issue, execlude the object that make the cycle using `@ToString.Exclude`                                                                                                                 |
| `@Builder`            | This enable us to create object step by step which is useful for complex objects.<br>Use `@Builder.Default` before fields that you sure that it will exists even if you didn't intialized by your self. Ex `private Set<Address> addresses = new HashSet<>();`<br>set before it that default annotation or you will get null pointer exception. |
| `@EqualsAndHashCode`  | This annotation on a class will generate implementations of both methods using all non-static, non-transient fields by default. You can customize which fields are included or excluded using parameters or other annotations.                                                                                                                  |
| `@Data`               | - This is a convenience shortcut annotation that bundles `@ToString`, `@EqualsAndHashCode`, `@Getter`, `@Setter`, and `@RequiredArgsConstructor` into a single annotation, generating all this boilerplate code for a typical POJO (Plain Old Java Object).                                                                                     |
```xml
<dependency>  
    <groupId>org.projectlombok</groupId>  
    <artifactId>lombok</artifactId>  
    <version>1.18.38</version>  
</dependency>
```

---
### Examples
#### Getters and setters
```java
@Getter
@Setter
@AllArgsConstructor
public class User{}
```

#### Builder
```java
@Builder
public class User{}


// Creating User object
var user = User.builder()
			.name("John")
			.password("password")
			.email("abc@gmail.com")
			.build();
```