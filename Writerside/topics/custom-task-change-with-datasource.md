# Create CustomTaskChange with Integration javax.sql.DataSource

How to create a CustomTaskChange with javax.sql.DataSource integration using Liquibase Kotlin DSL for custom execute and rollback operations.

```kotlin
import momosetkn.liquibase.kotlin.change.custom.core.ForwardOnlyTaskCustomChange
import momosetkn.liquibase.kotlin.change.custom.core.RollbackTaskCustomChange
import momosetkn.liquibase.kotlin.change.custom.core.addCustomChange
import momosetkn.liquibase.kotlin.change.custom.core.toJavaxSqlDataSource
import momosetkn.liquibase.kotlin.dsl.ChangeSetDsl

fun ChangeSetDsl.myCustomChange(
    confirmationMessage: String = "Executed myCustomChange.",
    rollback: ((yourpackage.MyDatabase) -> Unit)? = null,
    validate: (yourpackage.MyDatabase) -> ValidationErrors = { ValidationErrors() },
    execute: (yourpackage.MyDatabase) -> Unit,
) {
    val change = if (rollback != null) {
        val define = CustomRollbackableTaskChangeDefineImpl(
            executeBlock = execute,
            validateBlock = validate,
            rollbackBlock = rollback,
            confirmationMessage = confirmationMessage,
            transformDatabase = ::transformDatabase,
        )
        RollbackTaskCustomChange(define)
    } else {
        val define = CustomTaskChangeDefineImpl(
            executeBlock = execute,
            validateBlock = validate,
            confirmationMessage = confirmationMessage,
            transformDatabase = ::transformDatabase,
        )
        ForwardOnlyTaskCustomChange(define)
    }
    addCustomChange(change)
}

private fun transformDatabase(liquibaseDatabase: liquibase.database.Database): yourpackage.MyDatabase {
    val datasource = liquibaseDatabase.toJavaxSqlDataSource()
    val database = yourpackage.MyDatabaseFactory.create(datasource, liquibaseDatabase.shortName)
    return database
}
```

can use custom change in changeSet

```kotlin
changeSet(author = "user", id = "20241016-1000-10") {
    myCustomChange(
        // confirmationMessage is optional
        confirmationMessage = "message",
        execute = { db ->
            // your code
        },
        // rollback is optional
        rollback = { db ->
            // your code
        },
    )
}
```

