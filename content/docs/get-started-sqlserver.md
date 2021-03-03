+++
title = "SQL Server"
description = "SQL Server schema versioning and database migration with yuniql"
bref = ""
weight = 16
draft = false
toc = false
+++

#### Driver information

Connection string format, see https://www.connectionstrings.com/sql-server/
```shell
Server=<your-sqlserver-name>,1400;Database=<your-sqlserver-database-name>;
User Id=<your-sqlserver-user>;Password=<your-sqlserver-password>
```
|||
|---|---|
|Supported versions: |Sql Server 2017 and later, Azure SQL Database|
|Supports transactional DDL|Yes (Per session, per version, per statement)|
|Supports CSV bulk import|Yes|
|Supports batch statements|Yes, uses GO batch separator|
|Driver package|System.Data.SqlClient, see https://www.nuget.org/packages/System.Data.SqlClient

#### Getting started

Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql
```

Download samples for Sql Server. Advanced samples are also available here https://github.com/rdagumampan/yuniql/tree/master/samples

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-sqlserver-sample
```

Prepare your connection string in an environment variable. This sample uses SQL Server on Docker container and you may also use your local default instance. For more connection string samples, visit https://www.connectionstrings.com/sql-server/.

```shell
docker run -d -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=P@ssw0rd!" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest

SETX YUNIQL_PLATFORM "sqlserver"
SETX YUNIQL_WORKSPACE "c:\temp\yuniql\samples\basic-sqlserver-sample"
SETX YUNIQL_CONNECTION_STRING "Server=localhost,1400;Database=helloyuniql;User Id=SA;Password=P@ssw0rd!"
```

Apply migrations with `yuniql run`. Yuniql discovers the project directory, sorts all versions, creates the target database if it doesn't exist and runs all migration steps in the right order. Each migration step may include `.sql` files, directories, subdirectories, and csv files.

```shell
yuniql run --platform sqlserver -a --debug
yuniql list --platform sqlserver

Running yuniql v1.1.55 for windows-x64
Copyright 2019 (C) Rodel E. Dagumampan. Apache License v2.0
Visit https://yuniql.io for documentation & more samples

+---------------+----------------------+------------+---------------+----------------------+--------------+
| SchemaVersion | AppliedOnUtc         | Status     | AppliedByUser | AppliedByTool        | Duration     |
+---------------+----------------------+------------+---------------+----------------------+--------------+
| v0.00         | 2021-03-03 22:59:08Z | Successful | sa            | yuniql-cli v1.1.55.0 | 358 ms / 0 s |
| v0.01         | 2021-03-03 22:59:09Z | Successful | sa            | yuniql-cli v1.1.55.0 | 492 ms / 0 s |
| v0.02         | 2021-03-03 22:59:10Z | Successful | sa            | yuniql-cli v1.1.55.0 | 367 ms / 0 s |
+---------------+----------------------+------------+---------------+----------------------+--------------+
```

Verify results with your preferred SQL Client. A query with SSMS yields the following results.

![yuniql-sqlserver-migration](/images/get-started-sqlserver-01.png)

##### Known Issues and limitations

- Only supports username and password authentication on connection string. Advanced token-based autnentication have not been tested but could just work, please try and send us feedback.

##### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
