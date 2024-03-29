+++
title = "Baseline Your Database"
description  = "Prepare your database version v0.00. Learn strategies and tools to baseline your existing database."
bref  = "Prepare your database version v0.00. Learn strategies and tools to baseline your existing database."
weight = 403
draft = false
toc = false
+++

**DRAFT**

There are two industry approach to versioning your relational database. These are *Database-first* and *Code-first*. Arguments on both are valid and we have great literature online on [this debate](https://www.google.com/search?q=database+first+vs+code+first). 

Yuniql is a database-first migration engine. This is article is based on an assumption that you have preference on *Database-first* strategy.

#### Baselining strategies in database-first development
Versioning your database begins with a *Baseline*. A Baseline version, is the `v0.00` of your database schema and master data. A baseline version helps create full visibility of your schema evolution. That is from its conception to latest available change applied in DB. There are at least three approaches to baselining databases:
1. Visual model-first (new database projects)
2. Sql script-first (new database projects)
3. Sql script-dump from existing databases

##### Visual Model-First
Typically, we don't start our databases by hand-writing SQL scripts. Instead, we use advanced visual modelling tools such as SSMS Table Designer, SSDT, IDERA, Sparx EA and [similar tools](https://www.holistics.io/blog/top-5-free-database-diagram-design-tools) to create [Entity-Relationship Diagrams (ERD)](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model). Especially for larger DW and RDBMS projects, the scripts comes last as a result of good-enough ERD model. The scripts are then generated from the tool and this would make a sufficient starting point for baselining the db schema.

In this approach, you can generate all scripts from tool and place all scripts and directories inside `v0.00`. yuniql will discover and execute scripts in all directories and subdirectories.

```shell
yuniql init
cd v0.00
dir /O:N

10/21/2019  22:41    <DIR>          tables
10/21/2019  22:41    <DIR>          functions
10/21/2019  22:41    <DIR>          stored-procedures
10/21/2019  22:41    <DIR>          views
```

##### SqlScript-First
For smaller databases especially those attached to microservices, the model is relatively small and tables can be scripted on the go. Its simple and you can manually place all your scripts in order in `v0.00`. Scripts are executed in order by file name.

```shell
yuniql init
cd v0.00
dir /O:N

10/21/2019  22:41                   01-setup-tables.sql
10/21/2019  22:41                   02-setup-stored-procedures.sql
10/21/2019  22:41                   03-initialize-tables.sql
```

##### Sql Script Dump

For majority of use case, the database is already existing and running in Production. You can baseline your database by extracting and generating a baseline schema, supporting scripts and master data to produce a local database that is as-close as possible to Production but without the transaction data.

You can generate scripts from existing SQL Server databases using SSMS Export Scripts. Other database platforms management tools may have similar capability.

#### Baselining with YuniqlX

`yuniqlx baseline` is an experimental feature where we automate the script-generation of primary database objects and place results in `v0.00` of your migration project.  A command flow would look like this:

[Download latest `yuniqlx` build here](https://github.com/rdagumampan/yuniql-exploratory/releases/download/latest/yuniqlx-win-x64-latest.zip)
Setup your local workspace

```shell
yuniql init
```

Baseline your db

```shell
yuniqlx baseline -c <your-source-database-connection-string> -p <your-v0.00-directory-path>
```

Run your migration

```shell
yuniql run -c <your-source-database-connection-string> -a
```

>NOTE: `yuniqlx` is an experimental feature and released as separate package. Because it's not everyday that we do baseline plus its heavy references to Sql Server SMO, I don't want to make this part of every release.

#### Use your preferred way

The are certainly other ways to baseline your database. As long as they they can be validated in your target database, it can be organized in `v0.00` of your yuniql project and they will be discovered and executed. 

#### Learn further

* [Bulk Import CSV Master Data]({{< ref "/docs/bulk-import-csv-master-data.md" >}})
* [Use Token Replacement]({{< ref "/docs/token-replacement.md" >}})
* [Environment-aware Migration]({{< ref "/docs/environment-aware-scripts.md" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
