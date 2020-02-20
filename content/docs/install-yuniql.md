+++
title = "Intstall yuniql CLI"
description = "Install yuniql with choco, dotnet global tool, powershell, or direct download."
bref = "Install yuniql CLI from various distribution channels. Use Chocolatey package manager, dotnet global tool, powershell, nuget packages or download directly from relase repository."
weight = 2
draft = false
toc = false
+++

#### Install with Chocolatey
Downloads latest yuniql CLI with [Chocolatey](https://chocolatey.org/) package manager. See further instructions here https://chocolatey.org/install.
```shell
choco install yuniql --version 0.350.0
yuniql version
```

#### Install with .NET Core Global Tool
Installs latest yuniql CLI with .NET Core Global Tool. Requires .NET Core 3.0 SDK installed. See SDK here https://dotnet.microsoft.com/download/dotnet-core/3.1
```shell
dotnet tool install -g yuniql.cli
yuniql version
```

#### Install with Powershell
Downloads latest yuniql CLI and append into `PATH` environment variable.
```powershell
Invoke-WebRequest -Uri https://github.com/rdagumampan/yuniql/releases/download/latest/yuniql-cli-win-x64-latest.zip -OutFile  "c:\temp\yuniql-win-x64-latest.zip"
Expand-Archive "c:\temp\yuniql-win-x64-latest.zip" -DestinationPath "c:\temp\yuniql-cli"
$Env:Path += ";c:\temp\yuniql-cli"

yuniql version
```

#### Download `yuniql.exe` direct from source
Download zipped paclage containing `yuniql.exe` file and extract to your workspace directory.
https://github.com/rdagumampan/yuniql/releases/download/latest/yuniql-cli-win-x64-latest.zip

#### Install Nuget Package
##### Install on ASP.NET Core project
Use this for .NET Core WebApp and Worker App. Works only for .NET Core 3.0 and later. See how-to guide here https://github.com/rdagumampan/yuniql/wiki/How-to-run-migration-from-ASP.NET-Core.
```shell
dotnet add package Yuniql.AspNetCore
```

##### Install on .NET Core project
Use this for .NET Core Console App. Also usable for WebApp and Worker App but we recommend `Yuniql.AspNetCore` for that. Works only for .NET Core 3.0 and later. See how-to guide here https://github.com/rdagumampan/yuniql/wiki/How-to-run-migration-from-.NET-Core-Console-Application.
```shell
dotnet add package Yuniql.Core
```

### Learn further

* [Migrate via ASP.NET Core](https://yuniql.io/docs/migrate-via-aspnetcore-application/)
* [Migrate via Azure DevOps](https://yuniql.io/docs/migrate-via-azure-devops-pipelines/)
* [Migrate via Docker Container](https://yuniql.io/docs/migrate-via-docker-container/)
* [Migrate via Console Application](https://yuniql.io/docs/migrate-via-netcore-console-application/)
* [Bulk Import CSV Master Data](https://yuniql.io/docs/bulk-import-csv-master-data/)
* [Use Token Replacement](https://yuniql.io/docs/token-replacement/)
* [Environment-aware Migration](https://yuniql.io/docs/environment-aware-scripts/)

#### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).