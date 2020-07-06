+++
title = "PosgtreSql, MySql and Others"
description = "A quick-start guide to PostgreSql and other platforms. Install, run, verify."
bref = "A quick-start guide to working with PostgreSql and other platforms. Install, run, verify. 10-mins capped. Run these commands line by line via CLI tool like Bash, CMD and Powershell."
weight = 3
draft = false
toc = false
+++

Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql --version=1.0.1
```

Download samples for PostgreSql. Samples for sqlserver, mysql and other platforms are available here https://github.com/rdagumampan/yuniql/tree/master/samples

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-postgresql-sample
```

Prepare your connection string in an environment variable. This sample uses PostgreSql on Docker container. For more connection string samples, visit https://www.connectionstrings.com/postgresql.

```shell
docker run -d -e POSTGRES_USER=sa -e POSTGRES_PASSWORD=P@ssw0rd! -e POSTGRES_DB=helloyuniql -p 5432:5432 postgres
SETX YUNIQL_CONNECTION_STRING "Host=localhost;Port=5432;Username=sa;Password=P@ssw0rd!;Database=helloyuniql"
```

Apply migrations with `yuniql run` and specify the target platform with `--platform`. Yuniql discovers the project directory, sorts all versions, creates the target database if it doesn't exist and runs all migration steps in the right order. These includes `.sql` files, directories, subdirectories, and csv files.

```shell
yuniql run --platform postgresql -a --debug
yuniql list --platform postgresql

Running yuniql v1.0.1 for windows-x64
Copyright 2019 (C) Rodel E. Dagumampan. Apache License v2.0
Visit https://yuniql.io for documentation & more samples

+---------------+----------------------+------------+---------------+---------------------+
| SchemaVersion | AppliedOnUtc         | Status     | AppliedByUser | AppliedByTool       |
+---------------+----------------------+------------+---------------+---------------------+
| v0.00         | 2020-07-04 15:35:17Z | Successful | sa            | yuniql-cli v1.0.1.0 |
+---------------+----------------------+------------+---------------+---------------------+
```

Verify results with your preferred PostgreSql Client. A query with PgAdmin yields the following results.

![yuniql-evodb](/images/get-started-postgresql-01.png)

The latest build of yuniql supports SqlServer, PostgreSql, MySql and MariaDB. Integration tests are performed on instances hosted in Azure SQL Database, Amazon RDS and Google CloudSQL. See list of supported platforms [here]({{< ref "/docs/supported-platforms.md" >}})

#### Learn further

* [Supported Platforms]({{< ref "/docs/supported-platforms.md" >}})
* [Yuniql CLI Command Reference]({{< ref "/docs/yuniql-cli-command-reference.md" >}})
* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})

#### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
