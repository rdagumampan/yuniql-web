+++
title = "Oracle"
description = "Oracle schema versioning and database migration with yuniql"
bref = ""
weight = 16
draft = false
toc = false
+++

#### Driver information

Connection string format, see https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/
```shell
//for oracle xe instance
Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<your-oracle-server>)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=xe)));
User Id=<your-oracle-user>;Password=<your-oracle-user-password>;

//for oracle pluggable instance
Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<your-oracle-server>)(PORT=1521))
(CONNECT_DATA=(SERVICE_NAME=ORCLCDB.localdomain)));
User Id=<your-oracle-user>;Password=<your-oracle-user-password>;

//for oracle autonomous database on oracle cloud
Data Source=(description= (retry_count=20)(retry_delay=3)(address=(protocol=tcps)(port=1522)
(host=<your-adb-host-name>.oraclecloud.com))(connect_data=(service_name=<your-adb-database-name>))
(security=(<your-adb-server-certificate>)(MY_WALLET_DIRECTORY=<your-walltet-directory>)));
User Id=<your-adb-user>;Password=<your-adb-password>;

```
|||
|---|---|
|Supported versions: |Oracle 11g* and later|
|Supports transactional DDL|No (Per statement)|
|Supports CSV bulk import|Yes (Text fields only)|
|Supports batch statements|Yes, you can use `/` for commands and `;` for DDL statements|
|Driver package|ODP.NET, see https://www.oracle.com/database/technologies/appdev/dotnet/odp.html|

#### Getting started

Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql
```

Download samples for Oracle. For samples of other platforms, visit https://github.com/rdagumampan/yuniql/tree/master/samples

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-oracle-sample
```

Deploy a local oracle database container. For guidelines, visit https://medium.com/@firzhan/installing-oracle-12c-as-a-docker-container-44985b29bcae

```shell
docker login -u <dockerhub-user-id> -p <dockerhub-password>
docker run --rm -dit --name oracle12c  -p 1521:1521  store/oracle/database-enterprise:12.2.0.1-slim
docker ps

CONTAINER ID   IMAGE                                            COMMAND                  CREATED              STATUS                        PORTS                              NAMES
a8433450faf2   store/oracle/database-enterprise:12.2.0.1-slim   "/bin/sh -c '/bin/ba…"   About a minute ago   Up About a minute (healthy)   0.0.0.0:1521->1521/tcp, 5500/tcp   oracle12c
```

Verify connection with your SQL Developer or any oracle client using these connection information

```shell
hostname: localhost
port: 1521
service name: ORCLCDB.localdomain
username: sys
password: Oradoc_db1
```

Prepare your connection string and environment variables. You can also pass this directly as CLI parameters using `--platform oracle`, `-p <your-workspace>` and `-c <your-connection-string>`. For more connection string samples, visit https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/.

```shell
SETX YUNIQL_PLATFORM "oracle"
SETX YUNIQL_WORKSPACE "c:\temp\yuniql\samples\basic-oracle-sample"
SETX YUNIQL_CONNECTION_STRING "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=localhost)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=ORCLCDB.localdomain)));User Id=sys;Password=Oradoc_db1;DBA Privilege=SYSDBA;"
```

Verify connectivity with `yuniql check`. Here `yuniql` probes if your connectiong string works by sending `ping` requests to the server, verifying if the TCP port is open and attempting to connect to the target database.

```shell
yuniql check --debug

Running yuniql v1.0.0 for windows-x64
Copyright 2019 (C) Rodel E. Dagumampan. Apache License v2.0
Visit https://yuniql.io for documentation and working samples

INF   2022-01-08 12:17:02Z   Verifying ping connectivity to server/cluster localhost...
INF   2022-01-08 12:17:03Z   Ping connectivity to server/cluster localhost - Successful
INF   2022-01-08 12:17:03Z   Verifying port opening to server/cluster on localhost...
INF   2022-01-08 12:17:03Z   Port opening verification to server/cluster localhost - Successful
INF   2022-01-08 12:17:03Z   Verifying sql/odbc connectivity to master/catalog on localhost...
ERR   2022-01-08 12:17:05Z   Sql/odbc connectivity to master/catalog on localhost - Failed. Error message: ORA-12514: TNS:listener does not currently know of service requested in connect descriptor...
INF   2022-01-08 12:17:05Z   Verifying sql/odbc connectivity to database ORCLCDB.localdomain on localhost...
ERR   2022-01-08 12:17:07Z   Sql/odbc connectivity to database ORCLCDB.localdomain on localhost - Failed. Error message: ORA-12514: TNS:listener does not currently know of service requested in connect descriptor...

```

Keep trying `yuniql check` after few seconds until the container is ready to accept requests. Enable `--debug` for verbose and richer diagnostic messages.

```shell
yuniql check --debug

INF   2022-01-08 12:17:12Z   Verifying ping connectivity to server/cluster localhost...
INF   2022-01-08 12:17:12Z   Ping connectivity to server/cluster localhost - Successful
INF   2022-01-08 12:17:12Z   Verifying port opening to server/cluster on localhost...
INF   2022-01-08 12:17:12Z   Port opening verification to server/cluster localhost - Successful
INF   2022-01-08 12:17:12Z   Verifying sql/odbc connectivity to master/catalog on localhost...
INF   2022-01-08 12:17:13Z   Sql/odbc connectivity to master/catalog on localhost - Successful
INF   2022-01-08 12:17:13Z   Verifying sql/odbc connectivity to database ORCLCDB.localdomain on localhost...
INF   2022-01-08 12:17:13Z   Sql/odbc connectivity to database ORCLCDB.localdomain on localhost - Successful
```

Apply migrations with `yuniql run`. Here, `yuniql` discovers the project directory, sorts all versions, creates the target database if it doesn't exist and runs all migration steps in the right order. These includes `.sql` files, directories, subdirectories, and csv files.

```shell
yuniql run -a

WRN   2022-01-08 12:57:22Z   Target platform does not support for full transactional DDL operations. All operations will be executed with transaction mode = statement. When transaction mode is set to statement, each batch of sql statement does not participate in a shared transaction context. In the event of failure, the rollback attempt is limited to the individual batch of statement.

INF   2022-01-08 12:57:23Z   No explicit target version requested. We'll use latest available locally v0.02 on c:\temp\yuniql\samples\basic-oracle-sample.
INF   2022-01-08 12:57:23Z   Target database ORCLCDB.localdomain on localhost not yet configured for migration.
INF   2022-01-08 12:57:24Z   Configured database migration support for ORCLCDB.localdomain on localhost.
INF   2022-01-08 12:57:24Z   The schema version tracking table is up to date for ORCLCDB.localdomain on localhost.
INF   2022-01-08 12:57:24Z   Found 0 script files on c:\temp\yuniql\samples\basic-oracle-sample\_init
INF   2022-01-08 12:57:24Z   Found 0 bulk files on c:\temp\yuniql\samples\basic-oracle-sample\_init
INF   2022-01-08 12:57:24Z   Executed script files on c:\temp\yuniql\samples\basic-oracle-sample\_init
INF   2022-01-08 12:57:24Z   Found 0 script files on c:\temp\yuniql\samples\basic-oracle-sample\_pre
INF   2022-01-08 12:57:24Z   Found 0 bulk files on c:\temp\yuniql\samples\basic-oracle-sample\_pre
INF   2022-01-08 12:57:24Z   Executed script files on c:\temp\yuniql\samples\basic-oracle-sample\_pre
WRN   2022-01-08 12:57:24Z   Transaction is disabled for current session. This version migration run will be executed without explicit transaction context.
INF   2022-01-08 12:57:24Z   Found 2 script files on c:\temp\yuniql\samples\basic-oracle-sample\v0.00
  + 01-setup-tables.sql
  + 02-setup-data.sql
...
...
...
WRN   2022-01-08 12:57:25Z   Transaction has been committed before the end of the session. Please verify if all schema migrations has been successfully applied. If there was fault in the process, the database changes during migration process will be rolled back.
INF   2022-01-08 12:57:25Z   Schema migration completed successfuly on c:\temp\yuniql\samples\basic-oracle-sample.

```

Verify applied migration versions with `yuniql list`

```shell
yuniql list

+---------------+----------------------+------------+---------------+---------------------+--------------+
| SchemaVersion | AppliedOnUtc         | Status     | AppliedByUser | AppliedByTool       | Duration     |
+---------------+----------------------+------------+---------------+---------------------+--------------+
| v0.00         | 2022-01-08 12:57:24Z | Successful | SYS           | yuniql-cli v1.0.0.0 | 358 ms / 0 s |
| v0.01         | 2022-01-08 12:57:24Z | Successful | SYS           | yuniql-cli v1.0.0.0 | 135 ms / 0 s |
| v0.02         | 2022-01-08 12:57:25Z | Successful | SYS           | yuniql-cli v1.0.0.0 | 379 ms / 0 s |
+---------------+----------------------+------------+---------------+---------------------+--------------+

INF   2022-01-08 12:58:48Z   Listed all schema versions applied to database on c:\temp\yuniql\samples\basic-oracle-sample workspace.
For platforms not supporting full transactional DDL operations (ex. MySql, Snowflake, CockroachDB), unsuccessful migrations will show the status as Failed and you can look for FailedScriptPath and FailedScriptError in the schema version tracking table.

```

Verify results with your preferred Oracle Client. Here we show the results from Oracle SQL Developer. Open in new window/tab for better visibility.

![yuniql-evodb](/images/get-started-oracle-01.png)

Erase and clean your database with `yuniql erase`. Here, `yuniql` probes and executes all scripts inside `_erase` directory. Developer needs to handcraft the appropriate cleanup scripts when erasing database.

```shell
WRN   2022-01-08 12:55:57Z   Target platform does not support for full transactional DDL operations. 
All operations will be executed with transaction mode = statement. When transaction mode is set to statement, each batch of sql statement does not participate in a shared transaction context. 
In the event of failure, the rollback attempt is limited to the individual batch of statement.

INF   2022-01-08 12:55:57Z   Found 1 script files on c:\temp\yuniql\samples\basic-oracle-sample\_erase
  + 01-remove-tables.sql
INF   2022-01-08 12:55:59Z   Executed script file c:\temp\yuniql\samples\basic-oracle-sample\_erase\01-remove-tables.sql.
INF   2022-01-08 12:55:59Z   Executed script files on c:\temp\yuniql\samples\basic-oracle-sample\_erase
INF   2022-01-08 12:55:59Z   Schema erase completed successfuly on c:\temp\yuniql\samples\basic-oracle-sample.
```

##### Known Issues and limitations

- Bulk files are converted into multi-value insert and only supports string or varchar data types. If you need to support other data types you may create a script to move data from staging table into your final destination table. As alternative, you may opt out from using bulk files and instead prepare a seed script with values to load.
- While this is tested on a Oracle version that supports multi-tenant architecture, pluggable or container database, it was not specifically tested to make use of such feature during testing. We hope to improve this release further with more feedback for our users. Please send your comments and suggestions by creating an issue ticket.

##### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
