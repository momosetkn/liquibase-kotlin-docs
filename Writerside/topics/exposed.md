# Exposed-migration


## Install

Add bellow code your build.gradle.kts

```kotlin
dependencies {
    // liquibase-kotlin
    implementation("io.github.momosetkn:liquibase-kotlin-custom-exposed-migration-change:%liquibaseKotlinVersion%")
}
```

## Use customExposedMigrationChange

execute is required, rollback is optional.
the callback argument type is `org.jetbrains.exposed.sql.Database`.

<tabs>
<tab title="Compiled Kotlin">

```kotlin
import momosetkn.liquibase.kotlin.parser.KotlinCompiledDatabaseChangeLog
import momosetkn.liquibase.kotlin.change.custom.exposed.customExposedMigrationChange
import org.jetbrains.exposed.sql.SchemaUtils
import org.jetbrains.exposed.sql.Table
import org.jetbrains.exposed.sql.transactions.transaction

class CompiledDatabaseChangelog1 : KotlinCompiledDatabaseChangeLog({
    changeSet(author = "your_name", id = "20241007-2000-1") {
        // https://jetbrains.github.io/Exposed/table-definition.html#dsl-create-table
        val createdByExposed = object : Table("created_by_exposed") {
            val id = integer("id").autoIncrement()
            val name = varchar("name", 256)
            override val primaryKey = PrimaryKey(id)
        }
        customExposedMigrationChange(
            execute = { db ->
                transaction(db) {
                    SchemaUtils.create(createdByExposed)
                }
            },
            rollback = { db ->
                transaction(db) {
                    SchemaUtils.drop(createdByExposed)
                }
            },
        )
    }
})
```

</tab>
<tab title="Kotlin script">

<note>
Not require import below. because already imported.
<list>
    <li>org.jetbrains.exposed.sql.* </li>
    <li>org.jetbrains.exposed.sql.transactions.transaction </li>
</list>
</note>

```kotlin
databaseChangeLog {
    changeSet(author = "your_name", id = "20241007-2000-1") {
        // https://jetbrains.github.io/Exposed/table-definition.html#dsl-create-table
        val createdByExposed = object : Table("created_by_exposed") {
            val id = integer("id").autoIncrement()
            val name = varchar("name", 256)
            override val primaryKey = PrimaryKey(id)
        }
        customExposedMigrationChange(
            execute = { db ->
                transaction(db) {
                    SchemaUtils.create(createdByExposed)
                }
            },
            rollback = { db ->
                transaction(db) {
                    SchemaUtils.drop(createdByExposed)
                }
            },
        )
    }
}
```

</tab>
</tabs>
