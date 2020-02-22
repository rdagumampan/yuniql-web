+++
title = "How Yuniql Works"
description = "Understand the design principles, ways of working and internals of yuniql."
bref = "Understand the design principles, the ways of working and internals of yuniql."
weight = 10
draft = false
toc = false
+++

Yuniql is a Data Platform DevOps tool using migration-based and database-first delivery model. Migration-based means each changeset to the schema and seed data is set of carefully prepared scripts controlled with a version number. Database-first as it does not rely on application code to auto-generate the change scripts. Yuniql faciliates database DevOps with close integration with common DevOps tools such as Azure Pipelines and Docker.

![yuniql-evodb](/images/evodb-01.png)

At it's core, yuniql organizes the database migration steps in series of directories and files. Each directory represents an atomic version of your database executed in an all-or-nothing fashion. Utility directories are also created to cover pre and post migration activities. A typical starter project workspace looks like this.

```shell
yuniql init
```

```shell
Directory of C:\temp\sqlserver-sample
01/26/2020  09:22    <DIR>          .
01/26/2020  09:22    <DIR>          ..
11/03/2019  16:58                35 .gitignore
01/26/2020  09:30                67 Dockerfile
01/26/2020  09:22               782 README.md
11/10/2019  10:43    <DIR>          v0.00
11/10/2019  10:41    <DIR>          _draft
11/10/2019  10:54    <DIR>          _erase
11/10/2019  10:42    <DIR>          _init
11/10/2019  10:42    <DIR>          _post
11/10/2019  10:43    <DIR>          _prre
```

When `yuniql run` is issued the first time, it inspects the target database and creates required table to track the versions applied. All script files in `_init` directory and child directories will be executed only this time. The order of execution is as follows `_init`, `_pre`, `vx.xx`, `vxx.xx+N`, `_draft` , `_post`.

```shell
yuniql run -a

Executed scripts in _init directory
Executed scripts in _pre directory
Executed scripts in _v0.00 directory
Executed scripts in _draft directory
Executed scripts in _post directory
```

A successful first-run would have tracking the table filled-up this way.

```sql
SELECT * FROM __YuniqlDbVersion;

SequenceId	Version	AppliedOnUtc	    AppliedByUser	            AppliedByTool	AppliedByToolVersion
1	        v0.00	2020-02-21 06:10    DESKTOP-ULR8GDO\rdagumampan	yuniql-cli	    v0.350.0.0
```

As you create more versions over time, yuniql discovers all migrations left unapplied from source repostitory and apply to the target database. Assumming you have two more versions created locally `v1.00` and `v1.01`, the tracking table would have filled this way.

```shell
Directory of C:\temp\sqlserver-sample
...
11/10/2019  10:43    <DIR>          v0.00
11/10/2019  10:50    <DIR>          v1.00
11/10/2019  10:50    <DIR>          v1.01
...
```

```shell
yuniql run

Executed scripts in _v1.00 directory
Executed scripts in _v1.01 directory
```

```sql
SELECT * FROM __YuniqlDbVersion;

SequenceId	Version	AppliedOnUtc	    AppliedByUser	            AppliedByTool	AppliedByToolVersion
1	        v0.00	2020-02-21 06:10	DESKTOP-ULR8GDO\rdagumampan	yuniql-cli	    v0.350.0.0
2	        v1.00	2020-02-21 06:30	DESKTOP-ULR8GDO\rdagumampan	yuniql-cli	    v0.350.0.0
3	        v1.01	2020-02-21 06:30	DESKTOP-ULR8GDO\rdagumampan	yuniql-cli	    v0.350.0.0
```

When the version is ready for release, you can create deployment pipelines to release the changes to each environment. A typical YAML pipelines in Azure DevOps looks like this.


```yaml
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
    connectionString: '<your-connection-string>'
    workspacePath: '$(Build.SourcesDirectory)\sqlserver-sample'
    targetPlatform: 'SqlServer'
    additionalArguments: '--debug'
```



#### Advanced Options

There are situations when scripts are conditional to the environment they are executing. You may have scripts that fits in non-production environment but has to be changed when executed in production. Yuniql discovers environment-aware scripts during migration run. [More on this...]({{< ref "/docs/environment-aware-scripts.md" >}}). 

A baseline database includes both schema and data. While you can always write scripts to seed your data, it could be easier to bulk load them using CSV files. Yuniql discovers CSV files in the directories and load into tables bearing the name of the file. [More on this...]({{<ref "/docs/bulk-import-csv-master-data.md">}}).

When versioning an existing database, it may help to organize the scripts into collections of sub-directories. Yuniql discovers all directories and child directories, sort them, and execute based on order by name. [Tips and tricks...]({{<ref "/docs/tips-and-tricks.md#organize-in-sub-directories" >}})

#### Learn further

* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md#" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
