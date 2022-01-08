+++
title = "Snowflake"
description = "Snowflake schema versioning and database migration with yuniql"
bref = ""
weight = 306
draft = false
toc = false
+++

#### Driver information

Connection string format, see https://www.connectionstrings.com/postgresql/
```shell
host=<your-snowflake-host>com;account=<your-snowflake-account>;
user=<your-snowflake-user>;password<your-snowflake-password>;db=<your-snowflake-database-name>;schema=PUBLIC
```
|||
|---|---|
|Supported versions: |Snowflake 3.6.2* and later|
|Supports transactional DDL| No (Per statement)|
|Supports CSV bulk import|Yes|
|Supports batch statements|Yes, SQL statements are can be batch separated with GO word|
|Driver package|Yuniql.Snowflake.Data, see https://www.nuget.org/packages/Yuniql.Snowflake.Data|

#### Getting started

Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql
```

Download samples for Snowflake. For samples of other platforms, visit https://github.com/rdagumampan/yuniql/tree/master/samples

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-snowflake-sample
```

Prepare your connection string and environment variables. You can also pass this directly as CLI parameters.

```shell
SETX YUNIQL_PLATFORM "snowflake"
SETX YUNIQL_WORKSPACE "c:\temp\yuniql\samples\basic-snowflake-sample"
SETX YUNIQL_CONNECTION_STRING "host=<your-snowflake-host>com;account=<your-snowflake-account>;user=<your-snowflake-user>;password<your-snowflake-password>;db=<your-snowflake-database-name>;schema=PUBLIC"
```

Apply migrations with `yuniql run` and specify the target platform with `--platform`. Yuniql discovers the project directory, sorts all versions, creates the target database if it doesn't exist and runs all migration steps in the right order. These includes `.sql` files, directories, subdirectories, and csv files.

```shell
yuniql run --platform snowflake -a --debug
yuniql list --platform snowflake

Running yuniql v1.1.55 for windows-x64
Copyright 2019 (C) Rodel E. Dagumampan. Apache License v2.0
Visit https://yuniql.io for documentation & more samples

+---------------+----------------------+------------+---------------+----------------------+---------------+
| SchemaVersion | AppliedOnUtc         | Status     | AppliedByUser | AppliedByTool        | Duration      |
+---------------+----------------------+------------+---------------+----------------------+---------------+
| v0.00         | 2021-03-02 21:41:20Z | Successful | yuniqldev     | yuniql-cli v1.1.55.0 | 1569 ms / 1 s |
| v0.01         | 2021-03-02 21:41:22Z | Successful | yuniqldev     | yuniql-cli v1.1.55.0 | 1370 ms / 1 s |
| v0.02         | 2021-03-02 21:41:26Z | Successful | yuniqldev     | yuniql-cli v1.1.55.0 | 3277 ms / 3 s |
+---------------+----------------------+------------+---------------+----------------------+---------------+
```

Verify results with your Snowflake Management Plane. A query from Snowflake Editor yields the following results.

![yuniql-evodb](/images/get-started-snowflake-01.png)

##### Known Issues and limitations

- Only supports username and password authentication on connection string. Advanced token-based autnentication have not been tested but could just work, please try and send us feedback.
- Transaction mode default to Statement. Snowflake only supports Implicit Transaction for DDL statements. This means each DDL statement are executed as one atomic unit. If there's an open transaction prior to it, the transaction will be auto-committed and a new transaction will be created to cover the DDL statement. See https://docs.snowflake.com/en/sql-reference/transactions.html
- GO batch terminator. The .sql files can be prepared and batched with "GO" terminator. This is not official Snowflake batch terminator. We did this because the current semicolon ";" terminator parser seems need further work :). If you use "SELECT GET_DDL('DATABASE', 'HELLO_YUNIQL');" to dump schema from your database, please add "GO" at each DDL statement.

##### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
