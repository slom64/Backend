- artifact`maven-jar-plugin`
- when we generate jar file using class path, it give us executable that try to use link library to get other executables binaries. So our `jar` file can't run as standalone and we should include the dependecies with it.

```xml
<plugin>
	<artifactID>maven-jar-plugin</artifactID>
	<version>3.2.0</version>
	<configuration>
		<archive>
			<manifest>
				<addClasspath>true</addClasspath> <!-- Includes names of depenedencies that our project use. -->
				<mainClass>gov.iti.jets.MainApp</mainClass>
			</manifest>
		</archive>
	</configuration>
</plugin>
```

Now to add the other dependencis to our jar file so it can run with the dependencies run the following:
```sh
mvn dpenedency:copy-dependencies
```
or
```xml
<plugin>
	<artifactID>maven-dependeny-plugin</artifactID>
	<version>3.2.0</version>
	<executions>
		<execution>
			<phase>package</phase>
			<goals>
				<goal>copy-dependencies</goal>
			</goals>
		</execution>
	</executions>
	<configuration>
		<outputDirectory>${project.build.directory}</outputDirectory>
	</configuration>
</plugin>
```

---
