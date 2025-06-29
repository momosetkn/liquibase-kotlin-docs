# Home

<web-summary>
Discover how Liquibase-kotlin integrates Liquibase migrations with Kotlin, leveraging Kotlin-DSL and ORM support for streamlined, error-free compiled Kotlin development.
</web-summary>

[Liquibase-kotlin](https://github.com/momosetkn/liquibase-kotlin) was created with the aim of
integrating [liquibase](https://github.com/liquibase/liquibase) with kotlin.
This product will perform migration using Kotlin to unify to the Kotlin language.
The purpose of this product is to provide integration with Kotlin-DSL, Kotlin Liquibase client, and integration with
ORM.

The difference from the previous Kotlin DSL is that compiled Kotlin can be used.
I would like to recommend writing DSL in compiled Kotlin to take advantage of the assistance of an IDE and avoid runtime
error.

## Latest release

[![Maven Central](https://img.shields.io/maven-central/v/io.github.momosetkn/liquibase-kotlin-starter-compiled)](https://search.maven.org/artifact/io.github.momosetkn/liquibase-kotlin-starter-compiled)

## Supported Liquibase version

| Liquibase version                         | Supported liquibase-kotlin                                                                                                                                                                                                                         |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Latest master-version Liquibase           | [![test master-version Liquibase](https://github.com/momosetkn/liquibase-kotlin/actions/workflows/test-master-version-liquibase.yml/badge.svg)](https://github.com/momosetkn/liquibase-kotlin/actions/workflows/test-master-version-liquibase.yml) |
| [4.32.0](%liquibaseReleasesPage%/v4.30.0) | [4.32.0-0.10.0](%liquibaseKotlinReleasesPage%/4.32.0-0.10.0)                                                                                                                                                                                       |
| [4.32.0](%liquibaseReleasesPage%/v4.30.0) | [4.32.0-0.9.2](%liquibaseKotlinReleasesPage%/4.32.0-0.9.2)                                                                                                                                                                                         |
| [4.31.1](%liquibaseReleasesPage%/v4.30.0) | [4.31.0-0.9.1](%liquibaseKotlinReleasesPage%/4.31.0-0.9.1)                                                                                                                                                                                         |
| [4.31.0](%liquibaseReleasesPage%/v4.30.0) | [4.31.0-0.9.1](%liquibaseKotlinReleasesPage%/4.31.0-0.9.1)                                                                                                                                                                                         |
| [4.30.0](%liquibaseReleasesPage%/v4.30.0) | [4.30.0-0.9.0](%liquibaseKotlinReleasesPage%/4.30.0-0.9.0)                                                                                                                                                                                         |
| [4.29.2](%liquibaseReleasesPage%/v4.29.2) | [4.29.2-0.8.1](%liquibaseKotlinReleasesPage%/4.30.0-0.9.0), [4.29.2-0.8.0](%liquibaseKotlinReleasesPage%/4.29.2-0.8.0)                                                                                                                             |

## Version older than Liquibase 4.29.2 version

| Liquibase version                                                                     | Corresponding liquibase-kotlin                                                                                                                                                                                                                                                                  |
|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [4.29.1](%liquibaseReleasesPage%/v4.29.2)...[4.27.0](%liquibaseReleasesPage%/v4.27.0) | [4.29.2-0.8.1](%liquibaseKotlinReleasesPage%/4.29.2-0.8.1), [4.29.2-0.8.0](%liquibaseKotlinReleasesPage%/4.29.2-0.8.0)<br /><warning>Most of the features can be used, but cannot use updateCountSql command, updateToTagSql command</warning>                                                  |
| [4.26.0](%liquibaseReleasesPage%/v4.26.0)                                             | [4.29.2-0.8.1](%liquibaseKotlinReleasesPage%/4.29.2-0.8.1), [4.29.2-0.8.0](%liquibaseKotlinReleasesPage%/4.29.2-0.8.0)<br /><warning>Most of the features can be used, but cannot use updateCountSql command, updateToTagSql command. and cannot use dropDbclhistory args in dropAll.</warning> |
