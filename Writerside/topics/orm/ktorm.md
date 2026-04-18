# Ktorm


## install

Add bellow code your build.gradle.kts

```kotlin
dependencies {
    implementation("io.github.momosetkn:liquibase-kotlin-custom-ktorm-change")
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

## Configure org.ktorm.database.Database

<note>
Since a default implementation is provided, there is no need to customize it if it is not necessary.
</note>

override the `momosetkn.liquibase.kotlin.change.custom.ktorm.LiquibaseKtormConfig.provideDatabase`

example code

```kotlin
private val dialectMap = run {
    val loader = ServiceLoader.load(SqlDialect::class.java)
    loader
        .associateBy {
            val className = it::class.simpleName
            checkNotNull(className)
                .removeSuffix("Dialect").lowercase()
        }
}

fun provideDatabase(
    javaxSqlDataSource: javax.sql.DataSource,
    liquibaseDatabaseShortName: String
): Database {
    val dialect = getDialect(liquibaseDatabaseShortName)
    val database = Database.connect(
        javaxSqlDataSource,
        dialect
    )
    return database
}

internal fun getDialect(liquibaseDatabaseShortName: String): SqlDialect {
    return dialectMap[liquibaseDatabaseShortName] ?: StandardSqlDialect
}
momosetkn.liquibase.kotlin.change.custom.ktorm.LiquibaseKtormConfig.provideDatabase = ::provideDatabase
```
