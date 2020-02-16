+++
title = "Migrate via ASP.NET Core app"
description = "Run your database migration when your ASP.NET Core host service starts up"
weight = 10
draft = false
toc = false
bref = "Run your database migration when your ASP.NET Core host service starts up. This ensures that database is always at latest compatible state before operating the service.
+++

## Pre-requisites
- [.NET Core 3.0+ SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)
- [SQL Server or Azure SQL Database](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
- [Docker Client](https://www.docker.com/products/docker-desktop), if you choose SQL Server on Container

## Prepare your database

Deploy an SQL Server on Linux container or use your preferred instance.

```console
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=P@ssw0rd!" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```

## Run migration from .NET Core web app

Create new web app

```console
dotnet --version
3.0.100

dotnet new web -o aspnetcore-sample
cd aspnetcore-sample
```

Add `Yuniql.AspNetCore`

```console
dotnet add package Yuniql.AspNetCore
dotnet build
```

Copy sample database into `_db` directory in your project

```console
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql-aspnetcore
cd c:\temp\yuniql-aspnetcore\samples\basic-sqlserver-sample
```
	
Modify the `Configure` method of `Startup.cs`, add these lines
	
```csharp
using Yuniql.AspNetCore;
...
...

var traceService = new ConsoleTraceService { IsDebugEnabled = true };
app.UseYuniql(traceService, new YuniqlConfiguration
{
	WorkspacePath = Path.Combine(Environment.CurrentDirectory, "_db"),
	ConnectionString = "Server=localhost,1400;Database=yuniqldb;User Id=SA;Password=P@ssw0rd!",
	AutoCreateDatabase = true
});
```

Test run

```console
dotnet build
dotnet run --debug
```

## Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).