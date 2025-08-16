# Extension

## SlowLogChangeExecListener

Issuing a WARN log if ChangeSet is slow. With use slf4j.


### Install
Add the below dependency to your build.gradle.kts.

```kotlin
dependencies {
    implementation("io.github.momosetkn:liquibase-kotlin-extension")
}
```

### Setting

```kotlin
import momosetkn.liquibase.client.LiquibaseDatabaseFactory
import momosetkn.liquibase.extension.SlowLogChangeExecListener

fun main() {
    val database = LiquibaseDatabaseFactory.create(
        driver = "com.mysql.cj.jdbc.Driver",
        url = "jdbc:mysql://localhost:3306/test_db",
        username = "root",
        password = "",
    )
    val client = momosetkn.liquibase.client.LiquibaseClient(
        changeLogFile = /* your changeLog file path here */,
        database = database,
    ).also {
        // SlowLogChangeLog
        it.changeExecListener = SlowLogChangeExecListener()
        // or Setting option
        it.changeExecListener = SlowLogChangeExecListener(
            threshold = 1.seconds, // optional
            log = org.slf4j.LoggerFactory.getLogger("SlowLogChangeExecListener") // optional
        )
    }
    client.update()
}
```

If the threshold is exceeded, a warning will be logged at the WARN level.

`Slow ChangeSet: execute <FilePath>::<ID>::<Author> threshold-time is <ThresholdTime>ms`

If ChangeSet complete, the following elapsed log will be output at the DEBUG level.

`ChangeSet: execute <FilePath>::<ID>::<Author>  took <ElapsedTime>ms`
