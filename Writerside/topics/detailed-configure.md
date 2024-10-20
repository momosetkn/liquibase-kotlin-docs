# Detailed configure

## Kotlin-script

`kotlin-script-parser` module was Used global package function for IDE(IntelliJ IDEA) complement.
But, this is a global package, that is can call anywhere.
If you want to avoid this problem, Turn off with bellow code. exclude `liquibase-kotlin-script-parser-exterior` module.

```kotlin
    implementation("io.github.momosetkn:liquibase-kotlin-starter-script:$liquibaseKotlinVersion") {
        exclude(group = "io.github.momosetkn", module = "liquibase-kotlin-script-parser-exterior")
    }
```

## Liquibase concurrency

Supported version: 4.29.2-0.8.1 or later

Unfortunately, Liquibase was not thread-safe.
I chose not to fix the liquibase issue due to the liquibase complexity involved.
Instead, I resolved the thread-safety issue by using a new class loader

Can concurrency execute Liquibase by bellow set options.

```kotlin
momosetkn.liquibase.command.client.LiquibaseCommandClient.everyUseNewClassloader = true
momosetkn.liquibase.client.LiquibaseClient.everyUseNewClassloader = true
```
