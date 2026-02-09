

| Goal        | Description |
| ----------- | ----------- |
| compile     |             |
| testCompile |             |


```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.13.0</version>
            <configuration>
                <release>22</release>
            </configuration>
        </plugin>
    </plugins>
</build>
```