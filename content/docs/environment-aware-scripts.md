+++
title = "Run Environment-aware Migrations"
description = "Apply conditional migrations where different set of migrations is executed in the target environment."
bref = "Apply conditional migrations where different set of migrations is executed in the target environment."
weight = 9
draft = false
toc = false
+++

Environment-aware scripts are `sql` files that targets specific environment. For example,you may want to create big tables in Production with dedicated SQL Server file group, apply partition functions and partition schemes, and customize index fill factor but you don't need this in Development and Test environments. Another example is applying permissions by script that's different in Non-Production and Production environments.

#### Organize your repository
Any directory inside yuniql standard directories (`_init`,`_pre`,`_vxx.xx`,`_draft`,`_post`,`_erase`) that starts with underscore (`_`) represents an environment. To organize your repository, you may create `_dev`, `_test`, and `_prod` inside those directories or in any sub-directories.

Example #1: Organizes script by directory
```shell
v1.00
+ _development
    - setup_tables.sql
+ _test
    - setup_tables.sql
+ _staging
    - setup_tables.sql
+ _production
    - setup_tables.sql
```

Example #1: Organizes script in sub-directory
```shell
v1.00
+ setup-tables
  + _development
      - setup_tables.sql
  + _test
      - setup_tables.sql
  + _staging
      - setup_tables.sql
  + _production
      - setup_tables.sql
```

#### Run migrations
Repositories organized with environment-aware scripts requires that environment code is pass during migration. Yuniql will throw exception and fail the migration when no environment code passed into CLI or API.

```shell
yuniql run -a --environment DEVELOPMENT
yuniql run -a --environment TEST
yuniql run -a --environment STAGING
yuniql run -a --environment PRODUCTION
```
### Learn further

* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})
* [Use Token Replacement]({{< ref "/docs/token-replacement.md" >}})

#### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
