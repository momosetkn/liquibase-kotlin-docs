# Home

[Liquibase-kotlin](https://github.com/momosetkn/liquibase-kotlin) was created with the aim of integrating [liquibase](https://github.com/liquibase/liquibase) with kotlin.
This product will perform migration using Kotlin to unify to the Kotlin language.
The purpose of this product is to provide integration with Kotlin-DSL, Kotlin Liquibase client, and integration with ORM.

The difference from the previous Kotlin DSL is that compiled Kotlin can be used.
I would like to recommend writing DSL in compiled Kotlin to take advantage of the assistance of an IDE and avoid runtime error.

## Latest release

[![Maven Central](https://img.shields.io/maven-central/v/momosetkn/liquibase-kotlin-dsl)](https://search.maven.org/artifact/momosetkn/liquibase-kotlin-dsl)

## Support status Liquibase version

| Liquibase version                                                          | Supported liquibase-kotlin version                                                                                                                                                                                                                  |
|----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Latest SNAPSHOT](https://github.com/liquibase/liquibase/packages/1783578) | [![test snapshot-version Liquibase](https://github.com/momosetkn/liquibase-kotlin/actions/workflows/test-snapshot-liquibase.yml/badge.svg)](https://github.com/momosetkn/liquibase-kotlin/actions/workflows/test-snapshot-liquibase.yml)|
| [4.29.2](https://github.com/liquibase/liquibase/releases/tag/v4.29.2)      | [4.29.2-0.8.0](https://github.com/momosetkn/liquibase-kotlin/releases/tag/4.29.2-0.8.0)                                                                                                                                                             |
| [4.29.1](https://github.com/liquibase/liquibase/releases/tag/v4.29.1)      | [4.29.2-0.8.0](https://github.com/momosetkn/liquibase-kotlin/releases/tag/4.29.2-0.8.0)<br /><warning>Most of the features can be used, cannot use updateCountSql, updateToTagSql command</warning>                                                 |
| [4.29.0](https://github.com/liquibase/liquibase/releases/tag/v4.29.0)      | [4.29.2-0.8.0](https://github.com/momosetkn/liquibase-kotlin/releases/tag/4.29.2-0.8.0)<br /><warning>Most of the features can be used, cannot use updateCountSql, updateToTagSql command</warning>                                                 |
| [4.28.0](https://github.com/liquibase/liquibase/releases/tag/v4.28.0)      | [4.29.2-0.8.0](https://github.com/momosetkn/liquibase-kotlin/releases/tag/4.29.2-0.8.0)<br /><warning>Most of the features can be used, cannot use updateCountSql, updateToTagSql command</warning>                                                 |
| [4.27.0](https://github.com/liquibase/liquibase/releases/tag/v4.27.0)      | [4.29.2-0.8.0](https://github.com/momosetkn/liquibase-kotlin/releases/tag/4.29.2-0.8.0)<br /><warning>Most of the features can be used, cannot use updateCountSql, updateToTagSql command</warning>                                                 |
| [4.26.0](https://github.com/liquibase/liquibase/releases/tag/v4.26.0)      | [4.29.2-0.8.0](https://github.com/momosetkn/liquibase-kotlin/releases/tag/4.29.2-0.8.0)<br /><warning>Most of the features can be used, cannot use updateCountSql, updateToTagSql command. and cannot use dropDbclhistory args in dropAll.</warning> |
