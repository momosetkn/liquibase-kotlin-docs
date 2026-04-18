# Komapper-jdbc


## Install

Add bellow code your build.gradle.kts

```kotlin
dependencies {
    // liquibase-kotlin
    implementation("io.github.momosetkn:liquibase-kotlin-custom-komapper-jdbc-change")
}
```

## Use customKomapperJdbcChange

execute is required, rollback is optional.
the callback argument type is `org.komapper.jdbc.JdbcDatabase`.

<tabs>
<tab title="Compiled Kotlin">

```kotlin
import momosetkn.liquibase.kotlin.parser.KotlinCompiledDatabaseChangeLog
import momosetkn.liquibase.kotlin.change.custom.komapper.customKomapperJdbcChange
import org.komapper.core.dsl.QueryDsl

class CompiledDatabaseChangelog1 : KotlinCompiledDatabaseChangeLog({
    changeSet(author = "your_name", id = "20241007-2000-1") {
        customKomapperJdbcChange(
            execute = { db ->
                val query = QueryDsl.executeScript(
                    """
                    CREATE TABLE created_by_komapper (
                        id uuid NOT NULL,
                        name character varying(256)
                    );
                    """.trimIndent()
                )
                db.runQuery(query)
            },
            rollback = { db ->
                val query = QueryDsl.executeScript("DROP TABLE created_by_komapper")
                db.runQuery(query)
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
    <li>org.komapper.core.dsl.QueryDsl</li>
</list>
</note>

```kotlin
databaseChangeLog {
    changeSet(author = "your_name", id = "20241007-2000-1") {
        customKomapperJdbcChange(
            execute = { db ->
                val query = QueryDsl.executeScript(
                    """
                    CREATE TABLE created_by_komapper (
                        id uuid NOT NULL,
                        name character varying(256)
                    );
                    """.trimIndent()
                )
                db.runQuery(query)
            },
            rollback = { db ->
                val query = QueryDsl.executeScript("DROP TABLE created_by_komapper")
                db.runQuery(query)
            },
        )
    }
}
```

</tab>
</tabs>

## Configure org.komapper.jdbc.JdbcDatabase

<note>
Since a default implementation is provided, there is no need to customize it if it is not necessary.
</note>

override the `momosetkn.liquibase.kotlin.change.custom.komapper.LiquibaseKomapperJdbcConfig.provideJdbcDatabase`

example code

```kotlin
fun provideJdbcDatabase(
    javaxSqlDataSource: javax.sql.DataSource,
    liquibaseDatabaseShortName: String
): JdbcDatabase {
    return JdbcDatabase(javaxSqlDataSource, getJdbcDialect(liquibaseDatabaseShortName))
}

private fun getJdbcDialect(liquibaseDatabaseShortName: String): JdbcDialect {
    return runCatching {
        JdbcDialects.get(liquibaseDatabaseShortName)
    }.fold(
        onSuccess = { it },
        onFailure = {
            val loader = ServiceLoader.load(JdbcDialectFactory::class.java)
            val factory = loader.singleOrNull()
            checkNotNull(factory) {
                @Suppress("MaxLineLength")
                "I could not find the `org.komapper.jdbc.JdbcDialects` that should be used. Please set the `${this::class.qualifiedName}.provideJdbcDatabase`."
            }
            factory.create()
        }
    )
}
momosetkn.liquibase.kotlin.change.custom.komapper.LiquibaseKomapperJdbcConfig.provideJdbcDatabase = ::provideJdbcDatabase
```
