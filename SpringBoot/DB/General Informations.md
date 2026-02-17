- At the beginning we have two approches to create databases:
	- **Database First**: Which we create the database first in DBMS after that we start using database in our code.
	- **Model First**: Which we create database using code.

---
### JDBC
- **Java Database Connectivity**
- Its the standards API to connect to databases in java.
- It allow us to execute SQL queries directly
- **But** we have to manually manage everything.
```java
var connection = DriverManager.getConnection("https://database.com","username","password");  
var query = "SELECT id, name FROM users";  
var statement = connection.createStatement(); // create statement  
var result = statement.executeQuery(query);   // execute query
  
while(result.next()){  
    var id = result.getLong("id");  
    var name = result.getString("name");  
}  
result.close();  
statement.close();  
connection.close();
```


---
### JPA
- **Jakarta Persistence API**
- It is a specification for mapping java objects to database tables.
- We don't need to write raw SQL queries, We work directly with java objects.
- JPA it self doesn't has code its just an interface, So we use **Hibernate**, **OpenJPA**, **EclipseLink**
```java
@Entity
@Table(name="users")
public class User{
	@Id
	private Long id;
	private String name;
}

var user = new User("mosh");
var result = entityManager.persist(user);
entityMananger.close();
```

#### Hibernate
- An object/Relational Mapping "**ORM**".
- It provide us with implementation for JPA.
- It provide us aslo with
	- Caching
	- Hibernate Query Language
	- Automatic Schema generation

---
### Spring Data JPA
- its one of spring projects that is built on Hibernate JPA.
- Provides repository interfaces 
```java
public interface UserRepository extends CrudeRepostory<User, Long> {}
```