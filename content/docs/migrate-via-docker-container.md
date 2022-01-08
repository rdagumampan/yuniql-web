+++
title = "Migrate via Docker Container"
description = "Run your database migration thru a Docker container with Yuniql runtime."
bref = "Streamline your migration execution with Docker images with yuniql built-in. Use Docker CLI to run your migrations in any Docker compatible host."
weight = 353
draft = false
toc = false
+++ 

Run your database migration thru a Docker container. This is specially helpful on Linux environments and CI/CD pipelines running on Linux Agents as it facilitates your migration without having to worry any local installations or runtime dependencies. 

When you run `yuniql init` command, a baseline directory structure will be created automatically. This includes a ready-to-use Dockerfile.

<img src="/images/wiki-sample-sqlserverdb-01.png" width=700>

When you call `docker build`, we pull the base image containing the nightly build of `yuniql` and all of your local structure is copied into the image. When you call `docker run`, it executes a `yuniql run` inside the container and targets your desired server.

>NOTE: The container must have access to the target database and server. You may need to configure a firewall rule to accept login requests from the container hosts especially for cloud-based databases.

#### Pre-requisites
- [Docker Client](https://www.docker.com/products/docker-desktop), Linux mode
- [SQL Server or Azure SQL Database](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
 
#### Prepare database migration project

Install yuniql CLI.
Install yuniql CLI with Chocolatey or use alternative ways listed here [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}}).

```shell
choco install yuniql
```

Create new migration workspace.

```shell
md c:\temp\yuniql-docker
cd c:\temp\yuniql-docker

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

Check your Docker file and it should look like this. You can change the tag into specific release version of yuniql.

```dockerfile
FROM yuniql/yuniql:latest
COPY . ./db
```

Create first script file `01.setup_tables.sql` on `v0.00`.

```sql
CREATE TABLE regions (
    region_id INT IDENTITY(1,1) PRIMARY KEY,
    region_name VARCHAR (25) DEFAULT NULL
);
GO
```

Create first script file `02.setup_data.sql` on `v0.00`.

```sql
/*Data for the table regions */
SET IDENTITY_INSERT regions ON;
  
INSERT INTO regions(region_id,region_name) VALUES (1,'Europe');
INSERT INTO regions(region_id,region_name) VALUES (2,'Americas');
INSERT INTO regions(region_id,region_name) VALUES (3,'Asia');
INSERT INTO regions(region_id,region_name) VALUES (4,'Middle East and Africa');
 
SET IDENTITY_INSERT regions OFF;  
GO
```

Build local docker image.

```shell
docker build -t sqlserver-example .
```

Run migration from local docker.

```shell
docker run --rm sqlserver-example --platform sqlserver -d -a -c "<your-connection-string>"
```

Commit your project into git repository and use it as input in creating CI/CD pipelines.

#### Setup Azure DevOps Pipelines
The following pipelines runs `sqlserver-sample` project into Azure SQL Database. The samples are available on [Yuniql GitHub repository](https://github.com/rdagumampan/yuniql/tree/master/samples/sqlserver-sample). You may clone the repo to test the pipelines.

<img src="/images/dockerized-migration-03.png">

##### Agent task: docker build
```yaml
steps:
- task: Docker@2
  displayName: 'docker build'
  inputs:
    command: build
    Dockerfile: '$(System.DefaultWorkingDirectory)/_rdagumampan_yuniql/samples/basic-sqlserver-sample/Dockerfile'
    arguments: '-t sqlserver-example'
    addPipelineData: false
```
<img src="/images/dockerized-migration-01.png">

##### Agent task: docker run

```yaml
variables:
  AzSqlDemoDatabase: '<YOUR-SQLDATABASE-CONNECTIONSTRING>'

steps:
- task: Docker@2
  displayName: 'docker run'
  inputs:
    command: run
    arguments: 'sqlserver-example --platform sqlserver -d -a -c "$(AzSqlDemoDatabase)"'
    addPipelineData: false
```

<img src="/images/dockerized-migration-02.png">

#### Private Container Registry

If your company security policy disallows pulling images DockerHub, you may build the base image internally by cloning the GitHub repo and building docker images locally. The docker files are multi-staged. Please refer to `dockerfile.multi-stage-linux-x64` and `dockerfile.multi-stage-win-x64`.

Build images and push to your preferred registry

```shell
docker build -t yuniql -f dockerfile.multi-stage-linux-x64 .
docker tag yuniql:latest yuniql/yuniql:linux-x64-latest

docker login -u="%DOCKERHUB_USERNAME%" -p="%DOCKERHUB_PASSWORD%"
docker push yuniql:linux-x64-latest
```

```shell
docker build -t yuniql -f dockerfile.multi-stage-win-x64 .
docker tag yuniql:latest yuniql/yuniql:win-x64-latest

docker login -u="%DOCKERHUB_USERNAME%" -p="%DOCKERHUB_PASSWORD%"
docker push yuniql:win-x64-latest
```

Modify the Docker file of your database project

```dockerfile
#FROM yuniql/yuniql:linux-x64-latest
FROM <your-internal-repository>/yuniql:linux-x64-latest
COPY . ./db
```

#### Learn further

* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})
* [Yuniql CLI Command Reference]({{< ref "/docs/yuniql-cli-command-reference.md" >}})
* [Bulk Import CSV Master Data]({{< ref "/docs/bulk-import-csv-master-data.md" >}})
* [Use Token Replacement]({{< ref "/docs/token-replacement.md" >}})
* [Environment-aware Migration]({{< ref "/docs/environment-aware-scripts.md" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).