+++
title = "Get started!"
description = "Kick-start your database devops with yuniql CLI. Done in 10 mins max."
bref = "Yuniql allows developers, data engineers and DBAs to run migration steps from CLI, Azure DevOps and Docker. This is an express guide to using yuniql CLI. Run these commands line by line via Command Prompt (CMD)."
weight = 2
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
cd c:\temp\yuniql-getstarted\samples\basic-sqlserver-sample
```

#### Prepare connection
Set your db connection string in an environment variable. This demo uses local SQL Server instance. For more connection string samples, visit https://www.connectionstrings.com/sql-server/.

```shell
SETX YUNIQL_CONNECTION_STRING "Server=.\;Database=helloyuniql;Trusted_Connection=True;"
```

#### Run migration<br>
Apply migrations with `yuniql run`. Yuniql discovers the project directory, creates the target database if it doesn't exist and runs all migration steps in the order they are listed. These includes `.sql` files, directories, subdirectories, and csv files. Tokens are also replaced via `-k` parameters.
```shell
cd c:\temp\yuniql-getstarted\samples\basic-sqlserver-sample

yuniql run -a
yuniql info

Version         Created                         CreatedBy
v0.00           2019-11-03T16:29:36.0130000     DESKTOP-ULR8GDO\rdagumampan
```

#### Verify results<br>
Query tables with SSMS or your preferred SQL client

![yuniql-sqlserver-migration](/images/get-started-sqlserver.png)

#### Supported Platforms
The latest build supports SqlServer, PostgreSql and MySql. These have been verified to run on latest version hosted in Azure SQL Database, Amazon RDS and Google CloudSQL.

#### Learn further

* [Yuniql CLI Command Reference]({{< ref "/docs/yuniql-cli-command-reference.md" >}})
* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})

### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
