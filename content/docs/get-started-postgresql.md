+++
title = "Work with PosgtreSql and Others!"
description = "A quick-start guide to PostgreSql and other platforms. Install, run, verify."
bref = "A quick-start guide to working with PostgreSql and other platforms. Install, run, verify. 10-mins capped. Run these commands line by line via Command Prompt (CMD)."
weight = 3
draft = false
toc = false
+++

#### Install yuniql
Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql
```

#### Download samples
Clone yuniql repo and play with several samples for sqlserver, postgresql, mysql and other platforms. Find your samples here https://github.com/rdagumampan/yuniql/tree/master/samples
```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql-getstarted
cd c:\temp\yuniql-getstarted\samples\basic-postgresql-sample
```

#### Prepare connection
Set your db connection string in environment variable. This demo uses PostgreSql on docker container. For more connection string samples, visit https://www.connectionstrings.com/postgresql.

```shell
docker run -e POSTGRES_USER=sa -e POSTGRES_PASSWORD=P@ssw0rd! -e POSTGRES_DB=yuniqldb -p 5432:5432 postgres
SETX YUNIQL_CONNECTION_STRING "Host=localhost;Port=5432;Username=sa;Password=P@ssw0rd!;Database=yuniqldb"
```

#### Run migration

The following commands `yuniql` to discover the project directory, creates the target database if it doesn't exist and runs all migration steps in the order they are listed. These includes `.sql` files, directories, subdirectories, and csv files.

```shell
cd c:\temp\yuniql-getstarted\samples\basic-postgresql-sample

yuniql run --platform postgresql -a
yuniql info --platform postgresql

Version         Created                         CreatedBy
v0.00           2019-11-03T16:29:36.0130000     DESKTOP-ULR8GDO\rdagumampan
```

#### Verify results

Query tables with pgAdmin or your preferred PostgreSql client

![yuniql-evodb](/images/get-started-postgresql-01.png)

#### Supported Platforms

The latest build supports SqlServer, PostgreSql and MySql. These have been verified to run on latest version hosted in Azure SQL Database, Amazon RDS and Google CloudSQL.

### Learn further

* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})

### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
