# Ktorm


## install

Add bellow code your build.gradle.kts

```kotlin
dependencies {
    implementation("io.github.momosetkn:liquibase-kotlin-custom-ktorm-change:%liquibaseKotlinVersion%")
}
```

## Use customKtormChange

execute is required, rollback is optional.
the callback argument type is `org.ktorm.database.Database`.

<tabs>
<tab title="Compiled Kotlin">

```kotlin
import momosetkn.liquibase.kotlin.parser.KotlinCompiledDatabaseChangeLog
import momosetkn.liquibase.kotlin.change.custom.ktorm.customKtormChange

class CompiledDatabaseChangelog1 : KotlinCompiledDatabaseChangeLog({
    changeSet(author = "your_name", id = "20241007-2000-1") {
        customKtormChange(
            execute = { db ->
                val query = """
                     CREATE TABLE created_by_ktorm (
                        id uuid NOT NULL,
                        name character varying(256)
                    );
                """.trimIndent()
                db.useConnection { conn ->
                    conn.createStatement().execute(query)
                }
            },
            rollback = { db ->
                val query = "DROP TABLE created_by_ktorm"
                db.useConnection { conn ->
                    conn.createStatement().execute(query)
                }
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
        customKtormChange(
            execute = { db ->
                val query = """
                     CREATE TABLE created_by_ktorm (
                        id uuid NOT NULL,
                        name character varying(256)
                    );
                """.trimIndent()
                db.useConnection { conn ->
                    conn.createStatement().execute(query)
                }
            },
            rollback = { db ->
                val query = "DROP TABLE created_by_ktorm"
                db.useConnection { conn ->
                    conn.createStatement().execute(query)
                }
            },
        )
    }
}
```

</tab>
</tabs>