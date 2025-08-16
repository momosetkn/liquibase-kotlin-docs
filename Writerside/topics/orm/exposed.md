# Exposed-migration


## Install

Add bellow code your build.gradle.kts

```kotlin
dependencies {
    // liquibase-kotlin
    implementation("io.github.momosetkn:liquibase-kotlin-custom-exposed-migration-change")
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

## Configure org.jetbrains.exposed.sql.Database

<note>
Since a default implementation is provided, there is no need to customize it if it is not necessary.
</note>

override the `momosetkn.liquibase.kotlin.change.custom.exposed.LiquibaseExposedMigrationConfig.provideDatabase`

example code

```kotlin
fun provideDatabase(
    javaxSqlDataSource: javax.sql.DataSource,
    liquibaseDatabaseShortName: String
): Database {
    val dialect = getDialect(liquibaseDatabaseShortName)
    val db = Database.connect(
        datasource = javaxSqlDataSource,
        databaseConfig = dialect?.let {
            DatabaseConfig { explicitDialect = dialect }
        },
    )
    return db
}

internal fun getDialect(liquibaseDatabaseShortName: String): VendorDialect? {
    return when (liquibaseDatabaseShortName) {
        "h2" -> H2Dialect()
        "mariadb" -> MariaDBDialect()
        "mysql" -> MysqlDialect()
        "oracle" -> OracleDialect()
        "postgresql" -> PostgreSQLDialect()
        "sqlserver" -> SQLServerDialect()
        "sqlite" -> SQLiteDialect()
        else -> null
    }
}
momosetkn.liquibase.kotlin.change.custom.exposed.LiquibaseExposedMigrationConfig.provideDatabase = ::provideDatabase
```
k