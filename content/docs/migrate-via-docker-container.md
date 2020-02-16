+++
title = "Migrate via Docker Container"
description = "Run your database migration thru a Docker container with Yuniql runtime."
bref = "Run your database migration thru a docker container. This is specially helpful on Linux environments and CI/CD pipelines running on Linux Agents as it facilitates your migration without having to worry any local installations or runtime dependencies."
weight = 4
draft = false
toc = false
+++ 

When you run `yuniql init` command, a baseline directory structure will be created automatically. This includes a ready-to-use Dockerfile.

<img src="https://raw.githubusercontent.com/rdagumampan/yuniql/master/assets/wiki-sample-sqlserverdb-01.png" width=700>

When you call `docker build`, we pull the base image containing the nightly build of `yuniql` and all of your local structure is copied into the image. When you call `docker run`, it executes a `yuniql run` inside the container and targets your desired server.

>NOTE: The container must have access to the target database and server. You may need to configure a firewall rule to accept login requests from the container hosts especially for cloud-based databases.

#### Pre-requisites
- [Docker Client](https://www.docker.com/products/docker-desktop), Linux mode
- [SQL Server or Azure SQL Database](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
 
#### Prepare database migration project

Install yuniql CLI
Install yuniql CLI with Chocolatey or use alternative ways listed here https://github.com/rdagumampan/yuniql/wiki/Install-yuniql

```console
choco install yuniql --version 0.350.0
```
	
Create new migration workspace

```console
md c:\temp\yuniql-docker
cd yuniql-docker

yuniql init
dir /O:N

10/21/2019  22:41    <DIR>          _draft
10/21/2019  22:41    <DIR>          _erase
10/21/2019  22:41    <DIR>          _init
10/21/2019  22:41    <DIR>          _post
10/21/2019  22:41    <DIR>          _pre
10/21/2019  22:41    <DIR>          v0.00
10/21/2019  22:41                   Dockerfile
10/21/2019  22:41                   README.md
10/21/2019  22:41                   .gitignore
```

Create first script file `setup_tables.sql` on `v0.00`

```
CREATE TABLE [dbo].[Visitor](
	[VisitorID] [int] IDENTITY(1000,1) NOT NULL,
	[FirstName] [nvarchar](255) NULL,
	[LastName] [varchar](255) NULL,
	[Address] [nvarchar](255) NULL,
	[Email] [nvarchar](255) NULL
) ON [PRIMARY];
GO
```

Build docker image

```console
docker build -t sqlserver-example .
```

Run migration from docker

```console
docker run sqlserver-example -c "<your-connection-string>" -a
docker logs <your-container-id> -f
```

Commit your project into git and use it as input in creating CI/CD pipelines.

#### Setup Azure DevOps Pipelines
The following pipelines runs `sqlserver-sample` project into Azure SQL Database. The samples are available on [Yuniql GitHub repository](https://github.com/rdagumampan/yuniql/tree/master/samples/sqlserver-sample). You may clone the repo to test the pipelines.

<img src="https://raw.githubusercontent.com/rdagumampan/yuniql/master/assets/dockerized-migration-03.png">

##### Agent task: docker build
```
steps:
- task: Docker@2
  displayName: 'docker build'
  inputs:
    command: build
    Dockerfile: '$(System.DefaultWorkingDirectory)/_rdagumampan_yuniql/samples/basic-sqlserver-sample/Dockerfile'
    arguments: '-t sqlserver-example'
    addPipelineData: false
```
<img src="https://raw.githubusercontent.com/rdagumampan/yuniql/master/assets/dockerized-migration-01.png">

##### Agent task: docker run

```
variables:
  AzSqlDemoDatabase: '<YOUR-SQLDATABASE-CONNECTIONSTRING>'

steps:
- task: Docker@2
  displayName: 'docker run'
  inputs:
    command: run
    arguments: 'sqlserver-example -c "$(AzSqlDemoDatabase)" --debug'
    addPipelineData: false
```

<img src="https://raw.githubusercontent.com/rdagumampan/yuniql/master/assets/dockerized-migration-02.png">

#### Private Container Registry

If your company policy disallow pulling images DockerHub, you may build the image internally by cloning the GitHub repo and building docker images locally. The docker files are multi-staged. Please refer to `dockerfile.multi-stage-linux-x64` and `dockerfile.multi-stage-win-x64`.

Build images and push to your preferred registry

```console
docker build -t yuniql -f dockerfile.multi-stage-linux-x64 .
docker tag yuniql:latest rdagumampan/yuniql:linux-x64-latest

docker login -u="%DOCKERHUB_USERNAME%" -p="%DOCKERHUB_PASSWORD%"
docker push yuniql:linux-x64-latest
```

```console
docker build -t yuniql -f dockerfile.multi-stage-win-x64 .
docker tag yuniql:latest rdagumampan/yuniql:win-x64-latest

docker login -u="%DOCKERHUB_USERNAME%" -p="%DOCKERHUB_PASSWORD%"
docker push yuniql:win-x64-latest
```

Modify the Docker file of your database project

```
#FROM rdagumampan/yuniql:linux-x64-latest
FROM <your-internal-repository>/yuniql:linux-x64-latest
COPY . ./db
```

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).