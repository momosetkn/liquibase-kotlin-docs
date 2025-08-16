# ORM integration

There may be times when you want to perform data migration using ORM within migration by Liquibase.
By extending Liquibase's customChange, you can use ORM within Liquibase.

## Supported ORMs

The following ORMs are supported for integration with Liquibase:

- [Exposed](exposed.md) - JetBrains Exposed SQL framework
- [jOOQ](jooq.md) - Java Object Oriented Querying
- [Komapper](komapper.md) - Kotlin ORM and SQL mapper
- [Ktorm](ktorm.md) - Kotlin ORM framework
