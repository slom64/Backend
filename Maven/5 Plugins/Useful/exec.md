
| Goal | Description                                                                                                                         |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------- |
| exec | A goal to execute external progra                                                                                                   |
| java | Execute the supplied java class in the current VM with enclosing projects dependencies as classp                                    |
| he display information on `exec-maven-plugin`<br>call `mvn exec:help -Ddetail=true -Dgoal=<goal_name>` to display paramaters details. lay  |

- This is not maven artifact so it doesn't follow the standarad of maven artifacts.
```xml
<plugin>  
  <groupId>org.codehaus.mojo</groupId>  
  <artifactId>exec-maven-plugin</artifactId>  
  <version>3.1.0</version>  
  <configuration>    
	  <mainClass>gov.iti.jets.App</mainClass> 
  </configuration>  
</plugin>
```
Or using terminal
```sh
mvn exec:java -Dexec.mainClass=gov.iti.jets.App
```