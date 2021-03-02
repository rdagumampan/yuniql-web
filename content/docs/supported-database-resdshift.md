+++
title = "Redshift"
description = "Redshift database migration and schema versioning with yuniql"
bref = ""
weight = 16
draft = false
toc = false
+++

#### Driver information

Connection string format, see https://www.connectionstrings.com/postgresql/
```shell
Server=<your-redshift-server>;Port=5439;Database=<your-redshift-database-name>;
User Id=<your-redshift-user>;Password=<your-redshift-password>
```
|||
|---|---|
|Supported versions: |Redshift 1.0.2* or later|
|Supports transactional DDL|Full (Per session, per version, per statement)|
|Supports CSV bulk import|Yes|
|Supports batch statements|No, `.sql` files are executed as single batch|
|Driver package|npgsql, see https://www.npgsql.org/doc/index.html|

#### Getting started

Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql
```

Download samples for Redshift. For samples of other platforms, visit https://github.com/rdagumampan/yuniql/tree/master/samples

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-redshift-sample
```

Prepare your connection string and environment variables. You can also pass this directly as CLI parameters. For more connection string samples, visit https://www.connectionstrings.com/postgresql.

```shell
SETX YUNIQL_PLATFORM "redshift"
SETX YUNIQL_WORKSPACE "c:\temp\yuniql\samples\basic-redshift-sample"
SETX YUNIQL_CONNECTION_STRING "Server=<your-redshift-server>;Port=5439;Database=<your-redshift-database-name>;User Id=<your-redshift-user>;Password=<your-redshift-password>"
```

Apply migrations with `yuniql run` and specify the target platform with `--platform`. Yuniql discovers the project directory, sorts all versions, creates the target database if it doesn't exist and runs all migration steps in the right order. These includes `.sql` files, directories, subdirectories, and csv files.

```shell
yuniql run --platform redshift -a --debug
yuniql list --platform redshift

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

Verify results with your preferred Redshift Client. A query from Redshift Editor Plane yields the following results.

![yuniql-evodb](/images/get-started-redshift-01.png)

##### Known Issues and limitations

- Only supports username and password authentication on connection string. Advanced token-based autnentication have not been tested but could just work, please try and send us feedback.
- Driver used is primary for PostgreSql databases and limitation may emerge as Redshift continues to develop. See compatibility notes here https://www.npgsql.org/doc/compatibility.html#amazon-redshift

#### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
