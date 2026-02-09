https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html
```sh
.
├── pom.xml
├── target
	├── classes # Contain compiled classes .class from src folder.
	├── test-classes # compiled version of test classes
	├── surefire-reports # report output testing unit.
	├── application.jar # our application, mvn package
	├── maven-archiver  # store intermediate information that maven will use while building.
	├── maven-status    # store intermediate information.
	├── generated-sources      # output of plugins.
	├── generated-test-sources
└── src
    ├── main
    │   ├── java
    │   ├── resources
    │   ├── filters
    │   ├── webapp
    │   └── aspectj
    └── test
        ├── java
        ├── resources
        ├── filters
        └── aspectj
```

- `target` folder is the default path for any output of our maven building, reports,executables.. etc
- 