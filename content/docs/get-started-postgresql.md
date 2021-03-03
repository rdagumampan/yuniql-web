+++
title = "PosgtreSql"
description = "A quick-start guide to PostgreSql and other platforms. Install, run, verify."
bref = "A quick-start guide to working with PostgreSql and other platforms. Install, run, verify. 10-mins capped. Run these commands line by line via CLI tool like Bash, CMD and Powershell."
weight = 3
draft = false
toc = false
+++

#### Driver information

Connection string format, see https://www.connectionstrings.com/postgresql/
```shell
Server=<your-postgresql-server>;Port=5439;Database=<your-postgresql-database-name>;
User Id=<your-postgresql-user>;Password=<your-postgresql-password>
```
|||
|---|---|
|Supported versions: |PostgreSql v9.6 and later|
|Supports transactional DDL|Yes (Per session, per version, per statement)|
|Supports CSV bulk import|Yes|
|Supports batch statements|No, `.sql` files are executed as single batch|
|Driver package|npgsql, see https://www.npgsql.org/doc/index.html|

#### Getting started

Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql
```

Download samples for PostgreSql. For samples of other platforms, visit https://github.com/rdagumampan/yuniql/tree/master/samples

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-postgresql-sample
```

Prepare your connection string in an environment variable. This sample uses PostgreSql on Docker container. For more connection string samples, visit https://www.connectionstrings.com/postgresql.

```shell
docker run -d -e POSTGRES_USER=sa -e POSTGRES_PASSWORD=P@ssw0rd! -e POSTGRES_DB=helloyuniql -p 5432:5432 postgres

SETX YUNIQL_PLATFORM "postgresql"
SETX YUNIQL_WORKSPACE "c:\temp\yuniql\samples\basic-postgresql-sample"
SETX YUNIQL_CONNECTION_STRING "Host=localhost;Port=5432;Username=sa;Password=P@ssw0rd!;Database=helloyuniql"
```

Apply migrations with `yuniql run` and specify the target platform with `--platform`. Yuniql discovers the project directory, sorts all versions, creates the target database if it doesn't exist and runs all migration steps in the right order. These includes `.sql` files, directories, subdirectories, and csv files.

```shell
yuniql run --platform postgresql -a --debug
yuniql list --platform postgresql

Running yuniql v1.1.55 for windows-x64
Copyright 2019 (C) Rodel E. Dagumampan. Apache License v2.0
Visit https://yuniql.io for documentation & more samples

+---------------+----------------------+------------+---------------+----------------------+--------------+
| SchemaVersion | AppliedOnUtc         | Status     | AppliedByUser | AppliedByTool        | Duration     |
+---------------+----------------------+------------+---------------+----------------------+--------------+
| v0.00         | 2021-03-03 20:57:09Z | Successful | sa            | yuniql-cli v1.1.55.0 | 197 ms / 0 s |
| v0.01         | 2021-03-03 20:57:09Z | Successful | sa            | yuniql-cli v1.1.55.0 | 218 ms / 0 s |
| v0.02         | 2021-03-03 20:57:09Z | Successful | sa            | yuniql-cli v1.1.55.0 | 246 ms / 0 s |
+---------------+----------------------+------------+---------------+----------------------+--------------+
```

Verify results with your preferred PostgreSql Client. A query with PgAdmin yields the following results.

![yuniql-evodb](/images/get-started-postgresql-01.png)

##### Known Issues and limitations

- Only supports username and password authentication on connection string. Advanced token-based autnentication have not been tested but could just work, please try and send us feedback.
- Bulk import of CSV files works when the destination table is already committed. As work around, you can place the CSV files into separate minor version and apply migration with `--transaction-mode = version`.

##### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
