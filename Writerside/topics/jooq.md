5# Komapper-jdbc


## Install

Add bellow code your build.gradle.kts

```kotlin
dependencies {
    implementation("io.github.momosetkn:liquibase-kotlin-custom-jooq-change:%liquibaseKotlinVersion%")
}
```

## Use customKomapperJdbcChange

execute is required, rollback is optional.
the callback argument type is `org.jooq.DSLContext`.

<tabs>
<tab title="Compiled Kotlin">

```kotlin
import momosetkn.liquibase.kotlin.parser.KotlinCompiledDatabaseChangeLog
import momosetkn.liquibase.kotlin.change.custom.komapper.customJooqChange

class CompiledDatabaseChangelog1 : KotlinCompiledDatabaseChangeLog({
    changeSet(author = "your_name", id = "20241007-2000-1") {
        customJooqChange(
            execute = { db ->
                val query =
                    """
                CREATE TABLE created_by_komapper (
                    id uuid NOT NULL,
                    name character varying(256)
                );
                """.trimIndent()
                db.execute(query)
            },
            rollback = { db ->
                val query = "DROP TABLE created_by_komapper"
                db.execute(query)
            },
        )
    }
})
```

</tab>
<tab title="Kotlin script">

```kotlin
databaseChangeLog {
    changeSet(author = "your_name", id = "20241007-2000-1") {
        customJooqChange(
            execute = { db ->
                val query =
                    """
                CREATE TABLE created_by_komapper (
                    id uuid NOT NULL,
                    name character varying(256)
                );
                """.trimIndent()
                db.execute(query)
            },
            rollback = { db ->
                val query = "DROP TABLE created_by_komapper"
                db.execute(query)
            },
        )
    }
}
```

</tab>
</tabs>