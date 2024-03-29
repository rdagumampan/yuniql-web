+++
title = "Migrate via .NET Core Console App"
description = "Run your database migration when Console App starts."
bref = "Run your database migration when Console App starts. This ensures that database is always at latest compatible state before operating the service. This is made using `Yuniql.Core` nuget package. Package can be used for Worker and WebApp services."
weight = 354
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
docker run -d -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=P@ssw0rd!" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```

#### Run migration from .NET Core console app

Create new web app.

```shell
dotnet --version
3.0.100

dotnet new console -o console-sample
cd console-sample
```

Add `Yuniql.AspNetCore`.

```shell
dotnet add package Yuniql.Core
dotnet build
```

Copy sample database into `_db` directory in your project.

```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql
cd c:\temp\yuniql\samples\basic-sqlserver-sample
```
	
Modify the `Main` method of `Program.cs`.
	
```csharp
using Yuniql.Core;
...
...

static void Main(string[] args)
{
	//create custom trace message sinks, this can be your own logger framework
	var traceService = new ConsoleTraceService { IsDebugEnabled = true };

	//configure your migration run
	var configuration = Configuration.Instance;
	configuration.Platform = "sqlserver";
	configuration.Workspace = Path.Combine(Environment.CurrentDirectory, "_db");
	configuration.ConnectionString = "Server=localhost,1400;Database=helloyuniql;User Id=SA;Password=P@ssw0rd!";
	configuration.IsAutoCreateDatabase = true;

	//run migrations
	var migrationServiceFactory = new MigrationServiceFactory(traceService);
	var migrationService = migrationServiceFactory.Create();
	migrationService.Run();
}
```

Test .

```shell
dotnet build
dotnet run --debug
```

A working sample is available here for your reference https://github.com/rdagumampan/yuniql/tree/master/samples/sqlserver-console-sample.

#### Learn further

* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Yuniql CLI Command Reference]({{< ref "/docs/yuniql-cli-command-reference.md" >}})
* [Bulk Import CSV Master Data]({{< ref "/docs/bulk-import-csv-master-data.md" >}})
* [Use Token Replacement]({{< ref "/docs/token-replacement.md" >}})
* [Environment-aware Migration]({{< ref "/docs/environment-aware-scripts.md" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).