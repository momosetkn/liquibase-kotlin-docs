# Get started

[Liquibase-kotlin](https://momosetkn.github.io/liquibase-kotlin) was created with the aim of integrating [liquibase](https://github.com/liquibase/liquibase) with kotlin.
By using Kotlin for migration, unification is achieved.
This explains how-to migration by liquibase with Kotlin-DSL here.
<note>
You can choose DSL by Compiled Kotlin or kotlin-script.
I recommend Compiled Kotlin.
</note>

## Let's install

Update your build.gradle.kts.

<tabs>
<tab title="Compiled Kotlin">

```kotlin
repositories {
    mavenCentral()
    // Add below to repositories. because liquibase-kotlin is publish to jitpack.
    maven { url = URI("https://jitpack.io") }
}

dependencies {
    // liquibase
    implementation("org.liquibase:liquibase-core:%liquibaseVersion%")
    // liquibase-kotlin
    val liquibaseKotlinVersion = "%liquibaseKotlinVersion%"
    implementation("com.github.momosetkn.liquibase-kotlin:liquibase-kotlin-dsl:$liquibaseKotlinVersion")
    implementation("com.github.momosetkn.liquibase-kotlin:liquibase-kotlin-compiled-parser:$liquibaseKotlinVersion")
    implementation("com.github.momosetkn.liquibase-kotlin:liquibase-kotlin-compiled-serializer:$liquibaseKotlinVersion")
    implementation("com.github.momosetkn.liquibase-kotlin:liquibase-kotlin-client:$liquibaseKotlinVersion")
}
```

</tab>
<tab title="Kotlin script">

```kotlin
repositories {
    mavenCentral()
    // Add below to repositories. because liquibase-kotlin is publish to jitpack.
    maven { url = URI("https://jitpack.io") }
}

dependencies {
    // liquibase
    implementation("org.liquibase:liquibase-core:%liquibaseVersion%")
    // liquibase-kotlin
    val liquibaseKotlinVersion = "%liquibaseKotlinVersion%"
    implementation("com.github.momosetkn.liquibase-kotlin:liquibase-kotlin-dsl:$liquibaseKotlinVersion")
    implementation("com.github.momosetkn.liquibase-kotlin:liquibase-kotlin-script-parser:$liquibaseKotlinVersion")
    implementation("com.github.momosetkn.liquibase-kotlin:liquibase-kotlin-script-serializer:$liquibaseKotlinVersion")
    implementation("com.github.momosetkn.liquibase-kotlin:liquibase-kotlin-client:$liquibaseKotlinVersion")
}
```
</tab>
</tabs>

## Let's configure

liquibase-kotlin is provide client for kotlin.

`momosetkn.liquibase.client.configureLiquibase` methods is can global liquibase parameter.
If you want confirm each parameter. can confirm by [official-document](https://docs.liquibase.com/parameters/home.html).

```kotlin
import momosetkn.liquibase.client.LiquibaseDatabaseFactory
import momosetkn.liquibase.client.configureLiquibase

fun main() {
    configureLiquibase {
        global {
            general {
                showBanner = false
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
    // execute migrate
    client.update()
}
```

## Create changeLog

<tabs>
<tab title="Compiled Kotlin">

Create an extended ` momosetkn.liquibase.kotlin.parser.KotlinCompiledDatabaseChangeLog` class.
and add any changeSet.

```kotlin
// src/main/kotlin/example/DatabaseChangelog20241007Employee1.kt
package example

import momosetkn.liquibase.kotlin.parser.KotlinCompiledDatabaseChangeLog

class DatabaseChangelog20241007Employee1 : KotlinCompiledDatabaseChangeLog({
    changeSet(author = "your_name", id = "20241007-2000-1") {
        createTable(tableName = "employee") {
            column(name = "id", type = "UUID") {
                constraints(nullable = false, primaryKey = true)
            }
            column(name = "company_id", type = "UUID") {
                constraints(nullable = false)
            }
            column(name = "name", type = "VARCHAR(256)")
            column(name = "not_null_name", type = "VARCHAR(256)") {
                constraints(nullable = false)
            }
            column(name = "not_null_name2", type = "VARCHAR(256)") {
                constraints(nullable = false)
            }
        }
    }
})
```

specify class-name as changeLogFile to LiquibaseClient.

```kotlin
    val client = momosetkn.liquibase.client.LiquibaseClient(
    // set class-name
    changeLogFile = example.DatabaseChangelog20241007Employee1::class.qualifiedName!!,
    database = database,
)
client.update()
```

</tab>
<tab title="Kotlin script">

Put kts-file under the resource directory.

```kotlin
// src/main/resource/db/changelog/db.changelog-20241007-employee-1.kts
databaseChangeLog {
    changeSet(author = "your_name", id = "20241007-2000-1") {
        createTable(tableName = "employee") {
            column(name = "id", type = "UUID") {
                constraints(nullable = false, primaryKey = true)
            }
            column(name = "company_id", type = "UUID") {
                constraints(nullable = false)
            }
            column(name = "name", type = "VARCHAR(256)")
            column(name = "not_null_name", type = "VARCHAR(256)") {
                constraints(nullable = false)
            }
            column(name = "not_null_name2", type = "VARCHAR(256)") {
                constraints(nullable = false)
            }
        }
    }
}
```

specify file-path as changeLogFile to LiquibaseClient.

```kotlin
    val client = momosetkn.liquibase.client.LiquibaseClient(
        // set a file-path in the resource directory.
        changeLogFile = "db/changelog/db.changelog-20241007-employee-1.kts",
        database = database,
    )
    client.update()
```

</tab>
</tabs>

## Execute multiple changeLog with includeAll

<tabs>
<tab title="Compiled Kotlin">

```kotlin
// example) src/main/kotlin/example/DatabaseChangelogAll.kt
import momosetkn.liquibase.kotlin.parser.KotlinCompiledDatabaseChangeLog

class DatabaseChangelogAll : KotlinCompiledDatabaseChangeLog({
    // specify the name of the package that contains a changeLog class.
    includeAll("example.migration")
})

```

</tab>
<tab title="Kotlin script">

Put kts-file under the resource directory.

```kotlin
// example) src/main/resource/db/changelog/db.changelog-20241007-employee-1.kts
databaseChangeLog {
    // specify migration files directory
    includeAll("db/changelog/main")
}
```

</tab>
</tabs>

<tip title="How to include compiled-kotlin migration by kotlin-script migration">

Bellow example is include class `example.DatabaseChangelog20241007Employee1` in kotlin-script migration.
Should specify an extension is `.class`.

```kotlin
// kts(kotlin-script)
databaseChangeLog {
    // specify a compiled class-file file-path.
    include("example/migration/DatabaseChangelog20241007Employee1.class")
}
```
</tip>

## example project

[liquibase-kotlin-example](https://github.com/momosetkn/liquibase-kotlin-example)
