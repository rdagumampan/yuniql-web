+++
title = "MySql"
description = "MySql schema versioning and database migration with yuniql"
bref = ""
weight = 303
draft = false
toc = false
+++

#### Driver information

Connection string format, see https://www.connectionstrings.com/mysql/
```shell
Server=<your-postgresql-server>;Port=5439;Database=<your-postgresql-database-name>;
User Id=<your-postgresql-user>;Password=<your-postgresql-password>
```
|||
|---|---|
|Supported versions: |v5.7, v8.*, and later|
|Supports transactional DDL| No (Per statement)|
|Supports CSV bulk import|Yes|
|Supports batch statements|No, `.sql` files are executed as single batch|
|Driver package|MySqlConnect.NET, see https://github.com/mysql/mysql-connector-net|

#### Getting started

Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql
```

Download samples for MySql. For samples of other platforms, visit https://github.com/rdagumampan/yuniql/tree/master/samples

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-mysql-sample
```

Prepare your connection string in an environment variable. This sample uses MySql on Docker container. For more connection string samples, visit https://www.connectionstrings.com/mysql.

```shell
docker run -dit --name yuniql-mysql -e MYSQL_ROOT_PASSWORD=P@ssw0rd! -d -p 3306:3306 mysql:latest --default-authentication-plugin=mysql_native_password

SETX YUNIQL_PLATFORM "mysql"
SETX YUNIQL_WORKSPACE "c:\temp\yuniql\samples\basic-mysql-sample"
SETX YUNIQL_CONNECTION_STRING "Server=localhost;Port=3306;Database=helloyuniql;Uid=root;Pwd=P@ssw0rd!;"
```

Apply migrations with `yuniql run` and specify the target platform with `--platform`. Yuniql discovers the project directory, sorts all versions, creates the target database if it doesn't exist and runs all migration steps in the right order. These includes `.sql` files, directories, subdirectories, and csv files.

```shell
yuniql run --platform mysql -a --debug
yuniql list --platform mysql

Running yuniql v1.1.55 for windows-x64
Copyright 2019 (C) Rodel E. Dagumampan. Apache License v2.0
Visit https://yuniql.io for documentation & more samples

WRN   2021-03-03 22:23:54Z   Target platform does not support for full transactional DDL operations. 
All operations will be executed with transaction mode = statement. When transaction mode is set to statement, 
each batch of sql statement does not participate in a shared transaction context. In the event of failure, 
the rollback attempt is limited to the individual batch of statement.

+---------------+----------------------+------------+---------------+----------------------+---------------+
| SchemaVersion | AppliedOnUtc         | Status     | AppliedByUser | AppliedByTool        | Duration      |
+---------------+----------------------+------------+---------------+----------------------+---------------+
| v0.00         | 2021-03-03 22:23:45Z | Successful | root@%        | yuniql-cli v1.1.55.0 | 1248 ms / 1 s |
| v0.01         | 2021-03-03 22:23:46Z | Successful | root@%        | yuniql-cli v1.1.55.0 | 1269 ms / 1 s |
| v0.02         | 2021-03-03 22:23:49Z | Successful | root@%        | yuniql-cli v1.1.55.0 | 1903 ms / 1 s |
+---------------+----------------------+------------+---------------+----------------------+---------------+
```

Verify results with your preferred MySql Client. A query with yields the following results.

![yuniql-evodb](/images/get-started-mysql-01.png)

##### Known Issues and limitations

- Only supports username and password authentication on connection string. Advanced token-based autnentication have not been tested but could just work, please try and send us feedback.

##### Watch our short videos on youtube

{{< youtube juQkHb4GxRA >}}
<br/>

##### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
