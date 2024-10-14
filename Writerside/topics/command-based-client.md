# Command-based client

This explains how to use the liquibase command-based client.
This is something that has been replaced with a method from what was provided by the official Liquibase in the command line.

## install

Update your build.gradle.kts.

```kotlin
dependencies {
    implementation("io.github.momosetkn:liquibase-kotlin-command-client:%liquibaseKotlinVersion%")
}
```

## update command

```kotlin
import momosetkn.liquibase.command.client.LiquibaseDatabaseFactory

fun main() {
    val liquibaseClient = LiquibaseCommandClient {
        global {
            general {
                showBanner = false
                logLevel = "debug"
            }
        }
    }
    liquibaseClient.update(
        driver = "com.mysql.cj.jdbc.Driver",
        url = "jdbc:mysql://localhost:3306/test_db",
        username = "root",
        password = "",
        changelogFile = ExampleChangeLog::class.qualifiedName!!,
    )
}
```

If you want confirm each parameter. can confirm by [official-document](https://docs.liquibase.com/parameters/home.html).
