# New Spring Boot Axon Kotlin project

### Spring boot init

Either via [https://start.spring.io] or in IntelliJ, create new Spring Boot project.

In the majority of cases, add JPA and some database driver for token store, saga store, read models.

After initializing, modify properties

```
logging.level.com.example=DEBUG
spring.application.name=MyApp
```

### Upgrade Kotlin

```
<kotlin.version>1.3.11</kotlin.version>
```
See for latest e.g. [https://mvnrepository.com/artifact/org.jetbrains.kotlin/kotlin-stdlib]

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
<dependency>
    <groupId>com.fasterxml.jackson.module</groupId>
    <artifactId>jackson-module-kotlin</artifactId>
</dependency>
```

### Add logging

```
<kotlin-logging.version>1.6.22</kotlin-logging.version>

<dependency>
    <groupId>io.github.microutils</groupId>
    <artifactId>kotlin-logging</artifactId>
    <version>${kotlin-logging.version}</version>
</dependency>
```

See for latest version [https://mvnrepository.com/artifact/io.github.microutils/kotlin-logging]

### Now start working on some business logic

Start pattern for an aggregate:

```
import mu.KLogging
import org.axonframework.commandhandling.CommandHandler
import org.axonframework.eventsourcing.EventSourcingHandler
import org.axonframework.modelling.command.AggregateIdentifier
import org.axonframework.modelling.command.AggregateLifecycle.apply
import org.axonframework.modelling.command.TargetAggregateIdentifier
import org.axonframework.spring.stereotype.Aggregate
import java.util.*

data class XId(val value: UUID)
data class CreateXCmd(@TargetAggregateIdentifier val id: XId)
data class XCreatedEvt(val id: XId)

@Aggregate
internal class X() {

    companion object : KLogging()

    @AggregateIdentifier
    private lateinit var id: XId

    @CommandHandler
    constructor(cmd: CreateXCmd) : this() {
        logger.debug { "processing $cmd" }
        apply(XCreatedEvt(cmd.id))
    }

    @EventSourcingHandler
    fun on(evt: XCreatedEvt) {
        logger.debug { "applying $evt" }
        id = evt.id
    }

}
```