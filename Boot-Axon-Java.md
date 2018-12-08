# New Spring Boot Axon Java project

### Spring boot init

Either via [https://start.spring.io] or in IntelliJ, create new Spring Boot project.

In the majority of cases, add JPA and some database driver for token store, saga store, read models.

Add Lombok.

After initializing, modify properties

```
logging.level.com.example=DEBUG
spring.application.name=MyApp
```

### Add Axon stuff

```
<axon.version>4.0.3</axon.version>

<dependency>
    <groupId>org.axonframework</groupId>
    <artifactId>axon-spring-boot-starter</artifactId>
    <version>${axon.version}</version>
</dependency>
```

See for latest version [https://mvnrepository.com/artifact/org.axonframework/axon-spring-boot-starter]

### Serialize to JSON in all cases

```
axon.serializer.general=jackson

<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.module</groupId>
    <artifactId>jackson-module-parameter-names</artifactId>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jdk8</artifactId>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
</dependency>

File lombok.config (in root, with pom.xml):
lombok.noArgsConstructor.extraPrivate=true
```

### Now start working on some business logic

Typical patterns:

```
@Value 
public class CreateXCmd {
   @TargetAggregateIdentifier UUID id;
}

@Aggregate
@NoArgsContructor
@Slf4j
public class X {
```