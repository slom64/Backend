
| Annotation                                     | Description                                                                                                                                                                 |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `@JsonProperty("newName")`                     | Set the field name in json output, which makes it different from the field na                                                                                               |
| `@JsonInclude(JsonInclude.Include.NON_NULL)`   | If value is null, remove this key:value pair from the json outp                                                                                                             |
| `@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss" Change the output form, used like when we use<br>`@Mapping(target = "createAt", expression = "java(java.time.localDataTime.now())")`<br>which give bad output looking time ive  |


```java
public class UserDto {  
    private Long id;  
    private String name;  
    
    @JsonInclude(JsonInclude.Include.NON_NULL)  
    private String email;  
}
```