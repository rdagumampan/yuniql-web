+++
title = "Get started!"
description = "Kick-start your database devops with yuniql. Done in 10 mins max."
bref = "Yuniql allows developers, data engineers and DBAs to run migration steps from CLI, Azure DevOps and Docker. This is an express guide to using yuniql CLI. Run these commands line by line via Command Prompt (CMD) or Powershell."
weight = 3
draft = false
toc = false
+++

Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql
```

Download samples for Sql Server. Samples for sqlserver, postgresql and other platforms are available here https://github.com/rdagumampan/yuniql/tree/master/samples

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql-getstarted
cd c:\temp\yuniql-getstarted\samples\basic-sqlserver-sample
```

Prepare your connection string in an environment variable. This sample uses SQL Server on Docker container and you may also use your local default instance. For more connection string samples, visit https://www.connectionstrings.com/sql-server/.

```shell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=P@ssw0rd!" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest
SETX YUNIQL_CONNECTION_STRING "Server=localhost,1400;Database=helloyuniql;User Id=SA;Password=P@ssw0rd!"
```

Apply migrations with `yuniql run`. Yuniql discovers the project directory, sorts all versions, creates the target database if it doesn't exist and runs all migration steps in the right order. Each migration step may include `.sql` files, directories, subdirectories, and csv files.

```shell
yuniql run -a
yuniql info

Version         Created                         CreatedBy
v0.00           2019-11-03T16:29:36.0130000     DESKTOP-ULR8GDO\rdagumampan
```

Verify results with your preferred SQL Client. A query with SSMS yields the following results.

![yuniql-sqlserver-migration](/images/get-started-sqlserver.png)

The latest build of yuniql supports SqlServer, PostgreSql and MySql. Integration tests are performed on instances hosted in Azure SQL Database, Amazon RDS and Google CloudSQL. See list of supported platforms [here]({{< ref "/docs/build-status.md" >}})

#### Learn further

* [Yuniql CLI Command Reference]({{< ref "/docs/yuniql-cli-command-reference.md" >}})
* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})

#### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
