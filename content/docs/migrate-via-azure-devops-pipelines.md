+++
title = "Migrate via Azure DevOps Pipelines"
description = "Run your database migration from Azure DevOps and YAML pipelines."
bref = "Run your database migration from Azure DevOps Pipelines."
weight = 3
draft = false
toc = false
+++

Run your database migration from Azure DevOps Pipelines. The tasks downloads package and cache it for later execution just like how `Use .NET Core` or `Use Node` tasks works. Find Yuniql on [Azure DevOps MarketPlace](https://marketplace.visualstudio.com/items?itemName=rdagumampan.yuniql-azdevops-extensions). Install the DevOps Extension and add the following tasks.

<img align="center" src="https://github.com/rdagumampan/yuniql/raw/master/assets/wiki-az-devops-task-pipeline.png">

#### Pre-requisites

* Works only with Windows-based agents for now
* Requires a yuniql compliant directory structure. To create this structure you may [install yuniql-cli](https://github.com/rdagumampan/yuniql/wiki/Install-yuniql), issue `yuniql init`, commit to a git repository and use the repository as input artifact in the pipelines. You may also copy our [existing samples](https://github.com/rdagumampan/yuniql/tree/master/samples) for your target database platform and commit to your own repo.

#### Azure DevOps Pipelines (YAML)
This sample uses a SqlServer Database project available in GitHub and deploy the database into an Azure SQL Database. See https://github.com/rdagumampan/yuniql/tree/master/samples/basic-sqlserver-sample.

```
trigger:
- master

pool:
  vmImage: 'windows-latest'

steps:
- task: UseYUNIQLCLI@0
  inputs:
    version: 'latest'

- task: RunYUNIQLCLI@0
  inputs:
    version: 'latest'
    connectionString: 'Server=tcp:<AZ-SQLSERVER>,1433;Initial Catalog=<AZ-SQLDB>;User ID=<USERID>;Password=<PASSWORD>;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;'
    workspacePath: '$(Build.SourcesDirectory)\samples\basic-sqlserver-sample'
    targetPlatform: 'SqlServer'
    additionalArguments: '--debug'
```

This runs database migration with yuniql-cli.
* `version`: The version of Yuniql CLI. If omitted, the latest version of yuniql-cli is installed. Visit the [releases](https://github.com/rdagumampan/yuniql/releases) to get an appropriate version. 
* `connectionString`: The connection string to your target database server.
* `workspacePath`: The location of your version directories to run.
* `targetPlatform`: The target database platform. Default is SqlServer.
* `autoCreateDatabase`: When true, creates and configure the database in the target server for yuniql migrations.
* `targetVersion`: The maximum target database schema version to run to.
* `tokenKeyValuePair`: Token key/value pairs for token replacement.
* `additionalArguments`: Additional CLI arguments

#### Azure DevOps Pipelines (Classic)
##### Use YUNIQL CLI Task

![](https://rdagumampan.gallerycdn.vsassets.io/extensions/rdagumampan/yuniql-azdevops-extensions/0.56.0/1576914414829/images/screenshot-01.png)

This download and installs the yuniql-cli.
* `version`: The version of Yuniql CLI. If omitted, the latest version of yuniql-cli is installed. Visit the [releases](https://github.com/rdagumampan/yuniql/releases) to get an appropriate version. 

##### Run YUNIQL CLI Task

![](https://rdagumampan.gallerycdn.vsassets.io/extensions/rdagumampan/yuniql-azdevops-extensions/0.56.0/1576914414829/images/screenshot-02.png)

#### Verify YUNIQL CLI Task
Runs an uncommitted migration run. This performs a dry-run migration to verify if all works good should the versions be decided to be applied. Requires at least one successful run was made in the target database.

#### Erase YUNIQL CLI Task
Erases the target database objects (tables, procedures, functions, and others) using user defined clean-up scripts placed in `_erase` directory. Yuniql doesn't have automated erasure so user have to prepare the scope of erasure. 

>WARNING: This is helpful in Dev and Test. Be very careful and remove this task when cloning pipelines for Production!

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).