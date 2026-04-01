```xml
<properties>  
    <java.version>21</java.version>  
</properties>  
<dependencies>  
    <dependency>        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter</artifactId>  
    </dependency>    <dependency>        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-test</artifactId>  
        <scope>test</scope>  
    </dependency>    <dependency>        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-web</artifactId>  
    </dependency>    <dependency>        <artifactId>spring-boot-starter-data-jpa</artifactId>  
        <groupId>org.springframework.boot</groupId>  
    </dependency>    <dependency>        <artifactId>mysql-connector-j</artifactId>  
        <groupId>com.mysql</groupId>  
        <scope>runtime</scope>  
    </dependency>    <dependency>        <artifactId>flyway-core</artifactId>  
        <groupId>org.flywaydb</groupId>  
    </dependency>    <dependency>        <artifactId>flyway-mysql</artifactId>  
        <groupId>org.flywaydb</groupId>  
    </dependency>    <dependency>        <artifactId>lombok</artifactId>  
        <groupId>org.projectlombok</groupId>  
        <scope>annotationProcessor</scope>  
    </dependency>    <dependency>        <groupId>org.mapstruct</groupId>  
        <artifactId>mapstruct</artifactId>  
        <version>1.6.3</version>  
    </dependency>    <dependency>        <groupId>org.mapstruct</groupId>  
        <artifactId>mapstruct-processor</artifactId>  
        <version>1.6.3</version>  
    </dependency>    <dependency>        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-test</artifactId>  
        <scope>test</scope>  
    </dependency>  
  
    <dependency>        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-web</artifactId>  
    </dependency>  
    <dependency>        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-security</artifactId>  
    </dependency>  
    <dependency>        <groupId>org.thymeleaf.extras</groupId>  
        <artifactId>thymeleaf-extras-springsecurity6</artifactId>  
    </dependency>  
    <dependency>        <groupId>org.springframework.security</groupId>  
        <artifactId>spring-security-test</artifactId>  
        <scope>test</scope>  
    </dependency>  
    <dependency>        <groupId>io.jsonwebtoken</groupId>  
        <artifactId>jjwt-api</artifactId>  
        <version>0.12.6</version>  
    </dependency>    <dependency>        <groupId>io.jsonwebtoken</groupId>  
        <artifactId>jjwt-impl</artifactId>  
        <version>0.12.6</version>  
    </dependency>    <dependency>        <groupId>io.jsonwebtoken</groupId>  
        <artifactId>jjwt-jackson</artifactId>  
        <version>0.12.5</version>  
    </dependency>
<dependency>
	<artifactId>spring-boot-starter-validation</artifactId>
	<groupId>org.springframework.boot</groupId>
</dependency>    
  
</dependencies>  
  
<build>  
    <plugins>        <plugin>            <groupId>org.flywaydb</groupId>  
            <artifactId>flyway-maven-plugin</artifactId>  
            <version>10.15.0</version>  
            <configuration>                <url>jdbc:mysql://localhost:3306/store_api?createDatabaseIfNotExist=true</url>  
                <user></user>  
                <password></password>  
                <cleanDisabled>false</cleanDisabled>  
            </configuration>        </plugin>        <plugin>            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-compiler-plugin</artifactId>  
            <version>3.13.0</version>  
            <configuration>                <release>21</release>  
            </configuration>        </plugin>        <plugin>            <groupId>org.springframework.boot</groupId>  
            <artifactId>spring-boot-maven-plugin</artifactId>  
        </plugin>  
        <!-- Maven Surefire Plugin (Test Runner) -->  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-surefire-plugin</artifactId>  
            <configuration>                <useModulePath>false</useModulePath>  
  
                <!-- Verbose Test Output Equivalent -->  
                <printSummary>true</printSummary>  
                <redirectTestOutputToFile>false</redirectTestOutputToFile>  
                <forkCount>1</forkCount>  
                <reuseForks>true</reuseForks>  
            </configuration>        </plugin>
```