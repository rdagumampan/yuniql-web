+++
title = "Get started!"
description = "Kick-start your database devops with yuniql. Done in 10 mins max."
bref = "Express guide to versioning SqlServer and Azure Sql Database. Install, run, verify. 10-mins capped! Run line by line via CLI tool like Bash, CMD and Powershell."
weight = 2
draft = false
toc = false
+++

Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql --version=1.0.1
```

Download samples for Sql Server. Advanced samples are also available here https://github.com/rdagumampan/yuniql/tree/master/samples

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-sqlserver-sample
```

Prepare your connection string in an environment variable. This sample uses SQL Server on Docker container and you may also use your local default instance. For more connection string samples, visit https://www.connectionstrings.com/sql-server/.

```shell
docker run -d -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=P@ssw0rd!" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest
SETX YUNIQL_CONNECTION_STRING "Server=localhost,1400;Database=helloyuniql;User Id=SA;Password=P@ssw0rd!"
```

Apply migrations with `yuniql run`. Yuniql discovers the project directory, sorts all versions, creates the target database if it doesn't exist and runs all migration steps in the right order. Each migration step may include `.sql` files, directories, subdirectories, and csv files.

```shell
yuniql run -a --debug
yuniql list

Running yuniql v1.0.1 for windows-x64
Copyright 2019 (C) Rodel E. Dagumampan. Apache License v2.0
Visit https://yuniql.io for documentation & more samples

+---------------+----------------------+------------+---------------+---------------------+
| SchemaVersion | AppliedOnUtc         | Status     | AppliedByUser | AppliedByTool       |
+---------------+----------------------+------------+---------------+---------------------+
| v0.00         | 2020-07-04 15:35:17Z | Successful | sa            | yuniql-cli v1.0.1.0 |
+---------------+----------------------+------------+---------------+---------------------+
```

Verify results with your preferred SQL Client. A query with SSMS yields the following results.

![yuniql-sqlserver-migration](/images/get-started-sqlserver.png)

The latest build of yuniql supports SqlServer, PostgreSql, MySql and MariaDB. Integration tests are performed on instances hosted in Azure SQL Database, Amazon RDS and Google CloudSQL. See list of supported platforms [here]({{< ref "/docs/supported-platforms.md" >}})

#### Learn further

* [Yuniql CLI Command Reference]({{< ref "/docs/yuniql-cli-command-reference.md" >}})
* [Supported Platforms]({{< ref "/docs/supported-platforms.md" >}})
* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})

#### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
