+++
title = "Migrate via ASP.NET Core App"
description = "Run your database migration when your ASP.NET Core app starts up."
bref = "Run your database migration when your ASP.NET Core host service starts up. This ensures that database is always at latest compatible state before operating the service."
weight = 352
draft = false
toc = false
+++

#### Pre-requisites
- [.NET Core 3.0+ SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)
- [SQL Server or Azure SQL Database](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
- [Docker Client](https://www.docker.com/products/docker-desktop), if you choose SQL Server on Container

#### Prepare your database

Deploy an SQL Server on Linux container or use your preferred instance.

```shell
docker run -dit -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=P@ssw0rd!" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```

#### Run migration from .NET Core web app

Create new web app.

```shell
dotnet --version
3.0.100

dotnet new web -o aspnetcore-sample
cd aspnetcore-sample
```

Add `Yuniql.AspNetCore`.

```shell
dotnet add package Yuniql.AspNetCore
dotnet build
```

Copy sample database into `_db` directory in your project.

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-sqlserver-sample
```
	
Modify the `Configure` method of `Startup.cs`, add these lines.
	
```csharp
using Yuniql.AspNetCore;
...
...

//create custom trace message sinks, this can be your own logger framework
var traceService = new ConsoleTraceService { IsDebugEnabled = true };

//run migrations until latest version 
app.UseYuniql(traceService, new Yuniql.AspNetCore.Configuration
{
	Platform = "sqlserver",
	Workspace = Path.Combine(Environment.CurrentDirectory, "_db"),
	ConnectionString = "Server=localhost,1400;Database=helloyuniql;User Id=SA;Password=P@ssw0rd!",
	IsAutoCreateDatabase = true, IsDebug = true
});
```

Test run.

```shell
dotnet build
dotnet run --debug
```

A working sample is available here for your reference https://github.com/rdagumampan/yuniql/tree/master/samples/sqlserver-aspnetcore-sample.

#### Learn further

* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})
* [Yuniql CLI Command Reference]({{< ref "/docs/yuniql-cli-command-reference.md" >}})
* [Bulk Import CSV Master Data]({{< ref "/docs/bulk-import-csv-master-data.md" >}})
* [Use Token Replacement]({{< ref "/docs/token-replacement.md" >}})
* [Environment-aware Migration]({{< ref "/docs/environment-aware-scripts.md" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).