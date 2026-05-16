# Extend script-parser

<web-summary>
Extend the Liquibase Kotlin script parser by registering custom default imports through Java ServiceLoader integration.
</web-summary>

## Add default-import

default-imports is using `java.util.ServiceLoader`.

### Step1 - Create Service Provider File
`src/main/resources/META-INF/services/momosetkn.liquibase.kotlin.parser.KotlinScriptParserImports`

example
![Screenshot_from_2024-10-15_23-16-34.png](Screenshot_from_2024-10-15_23-16-34.png)

### Step2 - Create an implemented KotlinScriptParserImports class

Package and Class name is should be same to specified by step1.

```kotlin
package momosetkn

import momosetkn.liquibase.kotlin.parser.KotlinScriptParserImports

class CustomImports : KotlinScriptParserImports {
    override fun imports() = listOf(
        "momosetkn.hogehoge1",
        "momosetkn.hogehoge2",
    )
}


```
