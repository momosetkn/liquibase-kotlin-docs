# Client

This explains how to use the liquibase client.
It is a client implemented in Kotlin, it is easy to use.

## install

Update your build.gradle.kts.<br>
If you use [liquibase-kotlin-starter-compiled](https://central.sonatype.com/artifact/io.github.momosetkn/liquibase-kotlin-starter-compiled/%liquibaseKotlinVersion%) or [liquibase-kotlin-starter-script](https://central.sonatype.com/artifact/io.github.momosetkn/liquibase-kotlin-starter-script/%liquibaseKotlinVersion%), already installed.

```kotlin
dependencies {
    implementation("io.github.momosetkn:liquibase-kotlin-client)
}
```

## update command

```kotlin
import momosetkn.liquibase.client.LiquibaseDatabaseFactory
import momosetkn.liquibase.client.configureLiquibase

fun main() {
    configureLiquibase {
        global {
            general {
                showBanner = false
                logLevel = "debug"
            }
        }
    }
    // set your database
    val database = LiquibaseDatabaseFactory.create(
        driver = "com.mysql.cj.jdbc.Driver",
        url = "jdbc:mysql://localhost:3306/test_db",
        username = "root",
        password = "",
    )
    // set changelog and database
    val client = momosetkn.liquibase.client.LiquibaseClient(
        changeLogFile = /* your changeLog file path here */,
        database = database,
    )
    // example) use javax.sql.DataSource
    val client = momosetkn.liquibase.client.LiquibaseClient(
        changeLogFile = /* your changeLog file path here */,
        datasource = hikariDataSource,
    )
    // example) use java.sql.Connection
    val client = momosetkn.liquibase.client.LiquibaseClient(
        changeLogFile = /* your changeLog file path here */,
        connection = connection,
    )
    // execute migrate
    client.update()
}
```


liquibase-kotlin is provide client for kotlin.

`momosetkn.liquibase.client.configureLiquibase` methods is can global liquibase parameter.
If you want confirm each parameter. can confirm by [official-document](https://docs.liquibase.com/parameters/home.html).

## diffChangeLog command

```kotlin
import momosetkn.liquibase.client.LiquibaseDatabaseFactory
import momosetkn.liquibase.client.configureLiquibase

fun main() {
    configureLiquibase {
        global {
            general {
                showBanner = false
                logLevel = "debug"
            }
        }
    }
    // set your database
    val targetDatabase = LiquibaseDatabaseFactory.create(
        driver = "com.mysql.cj.jdbc.Driver",
        url = "jdbc:mysql://localhost:3306/target_test_db",
        username = "root",
        password = "",
    )
    val referanceDatabase = LiquibaseDatabaseFactory.create(
        driver = "com.mysql.cj.jdbc.Driver",
        url = "jdbc:mysql://localhost:3306/ref_test_db",
        username = "root",
        password = "",
    )
    // set changelog and database
    val client = momosetkn.liquibase.client.LiquibaseClient(
        changeLogFile = "src/main/kotlin/Example.kt",
        database = targetDatabase,
    )
    val ps = PrintStream(ByteArrayOutputStream())
    client.diffChangeLog(
        referenceDatabase = referenceDatabase,
        outputStream = ps,
    )
}
```

## generateChangeLog command

```kotlin
import momosetkn.liquibase.client.LiquibaseDatabaseFactory
import momosetkn.liquibase.client.configureLiquibase

fun main() {
    configureLiquibase {
        global {
            general {
                showBanner = false
                logLevel = "debug"
            }
        }
    }
    // set your database
    val database = LiquibaseDatabaseFactory.create(
        driver = "com.mysql.cj.jdbc.Driver",
        url = "jdbc:mysql://localhost:3306/target_test_db",
        username = "root",
        password = "",
    )
    // set changelog and database
    val client = momosetkn.liquibase.client.LiquibaseClient(
        changeLogFile = "src/main/kotlin/Example.kt",
        database = database,
    )
    val ps = PrintStream(ByteArrayOutputStream())
    client.generateChangeLog(
        outputStream = ps,
    )
}
```

