+++
title = "Install yuniql CLI"
description = "Install yuniql with choco, dotnet global tool, powershell, or direct download."
bref = "Install yuniql CLI from various distribution channels for Windows and Linux. Use Chocolatey, dotnet global tool, powershell, nuget packages or download directly."
weight = 103
draft = false
toc = false
+++

#### Download `yuniql.exe` directly from GitHub
Download zipped paclage containing `yuniql.exe` file and extract to your workspace directory.
https://github.com/rdagumampan/yuniql/releases/download/latest/yuniql-cli-win-x64-latest.zip

#### Install with choco package (windows-x64)
Downloads latest yuniql CLI with [Chocolatey](https://chocolatey.org/) package manager. See further instructions here https://chocolatey.org/install. Run these commands under Administrator mode.
```shell
choco install yuniql -y
yuniql version
```

#### Install with tar.gz package (linux-x64)
Install yuniql CLI on Linux. The package has been verified on Ubuntu 18.04+ and Debian on Windows Subsystem Linux (WSL). If you encounter this `Error: Couldn't find a valid ICU package installed on the system`, set global invariant variable `export DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1`.

```shell
cd /home
sudo curl https://github.com/rdagumampan/yuniql/releases/download/v1.1.55/yuniql-cli-linux-x64-latest.tar.gz -L -o yuniql.tar.gz
sudo tar -xvzf yuniql.tar.gz -C /bin

cd /bin
sudo chmod +x yuniql

yuniql version
```

#### Install with .NET Core Global Tool
Installs latest yuniql CLI with .NET Core Global Tool. Requires .NET Core 3.0 SDK installed. See SDK here https://dotnet.microsoft.com/download/dotnet-core/3.1
```shell
dotnet tool install -g yuniql.cli
yuniql version
```

#### Install with Powershell and PATH
Downloads latest yuniql CLI and append into `PATH` environment variable.
```powershell
Invoke-WebRequest -Uri https://github.com/rdagumampan/yuniql/releases/download/latest/yuniql-cli-win-x64-latest.zip -OutFile  "c:\temp\yuniql-win-x64-latest.zip"
Expand-Archive "c:\temp\yuniql-win-x64-latest.zip" -DestinationPath "c:\temp\yuniql-cli"
$Env:Path += ";c:\temp\yuniql-cli"

yuniql version
```

#### Install on ASP.NET Core project
Use this for .NET Core WebApp and Worker App. Works only for .NET Core 3.0 and later. See how-to guide here [{{< ref "/docs/migrate-via-aspnetcore-application.md" >}}]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}}).
```shell
dotnet add package Yuniql.AspNetCore
```

#### Install on any .NET Core project
Use this for .NET Core Console App. Also usable for WebApp and Worker App but we recommend `Yuniql.AspNetCore` for that. Works only for .NET Core 3.0 and later. See how-to guide here [{{< ref "/docs/migrate-via-netcore-console-application.md" >}}]({{< ref "/docs/migrate-via-netcore-console-application.md" >}}).
```shell
dotnet add package Yuniql.Core
```

#### Install Azure DevOps Extensions
Use this for running migration from YAML and classic pipelines. You can acquire free extensions from Azure Marketplace and install into your Azure DevOps organization https://marketplace.visualstudio.com/items?itemName=rdagumampan.yuniql-azdevops-extensions

![](https://rdagumampan.gallerycdn.vsassets.io/extensions/rdagumampan/yuniql-azdevops-extensions/0.56.0/1576914414829/images/screenshot-01.png)

#### Install with Docker
Use `yuniql` Docker base image to build and run your migration from a container. See instructions here [migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}}). You can can visit `yuniql` on [Docker Hub](https://hub.docker.com/repository/docker/yuniql/yuniql)

```shell
docker pull yuniql/yuniql:latest
docker pull yuniql/yuniql:linux-x64-latest
```

#### Learn further

* [Get started]({{< ref "/docs/get-started.md" >}})
* [Supported Platforms]({{< ref "/docs/supported-platforms.md" >}})
* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})

<!-- * [Yuniql CLI Command Reference]({{< ref "/docs/yuniql-cli-command-reference.md" >}})
* [Bulk Import CSV Master Data]({{< ref "/docs/bulk-import-csv-master-data.md" >}})
* [Use Token Replacement]({{< ref "/docs/token-replacement.md" >}})
* [Environment-aware Migration]({{< ref "/docs/environment-aware-scripts.md" >}}) -->

##### Watch our short videos on youtube

{{< youtube JaX6h-F-FZU >}}
<br/>

#### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).