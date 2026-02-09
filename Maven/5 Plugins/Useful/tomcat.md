- automates the deployment process. It takes the genenrated`war` file and deploy it on the server.

| Goal       | Disciption                                                             |
| ---------- | ---------------------------------------------------------------------- |
| `deploy`   | Deploy a servlet on tomcat server.                                     |
| `redeploy` | If the servlet exists before but you just updated it then use redeploy |


---

### Define
```xml
<plugin>
	<groupId>org.apache.tomcat.maven</groupId>
	<artifactId>tomcat7-maven-plugin</artifactId>
	<configuration>
		<username>admin</username> <!-- Configurations of tomcat server -->
		<password>admin</password> 
		<url>http://localhost:9090/manager/text/</url> <!-- Url of deployment -->
		<path>/myapp</path> <!-- the name of app when it is put on the server "Context path" -->
	</configuration>
</plugin>
```

---

### Usage
- deploy my servlet to tomcat server
```
mvn install tomcat7:deploy
```
- If you want to update the code then deploy again then use 
```
mvn tomcat7:redeploy
```