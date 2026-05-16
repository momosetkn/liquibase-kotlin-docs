# Detailed configure

<web-summary>
Configure Liquibase Kotlin script parsing in detail, including how to exclude the global script parser extension module.
</web-summary>

## Kotlin-script

`kotlin-script-parser` module was Used global package function for IDE(IntelliJ IDEA) complement.
But, this is a global package, that is can call anywhere.
If you want to avoid this problem, Turn off with bellow code. exclude `liquibase-kotlin-script-parser-exterior` module.

```kotlin
    implementation("io.github.momosetkn:liquibase-kotlin-starter-script:$liquibaseKotlinVersion") {
        exclude(group = "io.github.momosetkn", module = "liquibase-kotlin-script-parser-exterior")
    }
```
