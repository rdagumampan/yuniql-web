+++
title = "How Yuniql Works"
description = "Understand the design principles, ways of working and internals of yuniql."
bref = "yuniql is data platform devops tool. Schema versioning and database migration is one of its core capabilities. Understand its internals here."
weight = 10
draft = false
toc = false
+++

Yuniql organizes database schema version using series of ordinary directories and SQL files. Several utility directories and files are also included in default structure to support pre and post migration tasks. A typical baseline structure is described below and migration follows this order `_init [only on first-run]`,`_pre`,`v0.00`,`v.x.xx`,`vx.xx+1`,`_draft`,`_post`.

<img src="https://github.com/rdagumampan/yuniql/raw/master/assets/wiki-how-it-works-dir.png" width=700/>

Developers and DBAs can work in these sequence:

- `yuniql init` / initializes db project structure
- `yuniql vnext` / increment version
- `yuniql run` / run migrations
- `yuniql info` / show existing versions

### Yuniql CLI Command Reference

#### **`yuniql init`**
---
Creates baseline directory structure that serves as your database migration workspace. Commit this into your preferred source control platform such as `git`, `tfs vc` or `svn`. 

| Dir / File | Usage Description | Execution |
| --- | --- | --- |
| *_init* | Initialization scripts directory. <br/>Executed the first time `yuniql run` is issued|Executed once |
| *_pre* | Pre migration scripts directory. <br/>Executed every time before any version. | Every migration run |
| *v0.00* | Baseline scripts directory. <br/>Executed when `yuniql run`. | Executed once |
| *_draft* | Scripts in progress directory. <br/>Scripts that you are currently working and have not moved to specific version directory yet. <br/>Executed every time after the latest version. | Every migration run |
| *_post* | Post migration scripts directory. <br/>Executed every time and always the last batch to run. | Every migration run |
| *_erase* | Database cleanup scripts directory. <br/>Executed once only when `yuniql erase` is issued. | Executed on demand |
| *Dockerfile* | A template docker file to run your migration. <br/>Uses docker base images with `yuniql` installed.| Executed on `docker build` |
| *README.md* | A template README file.| |
| *.gitignore* | A template git ignore file to skip yuniql.exe from being committed.| |

#### **`yuniql vnext`**
---
Identifies the latest version locally and increment the minor version with the format `v{major}.{minor}`. The command just helps reduce human errors and this can also be done manually.

- `-m | --minor`

    Default option. Increments major version by creating `vx.xx+1` folder

- `-M | --major`

    Increments minor version by creating `vx+1.xx` folder

- `-f | --file <file-name.sql>`

    Creates an empty sql file in the created major or minor version

#### **`yuniql run`**
---
Inspects the target database and creates required table to track the versions. All script files in `_init` directory will be executed. The order of execution is as follows `_init`,`_pre`,`vx.xx`,`_draft`,`_post`. Several variations on how we can run migration are listed below.

 - `-p "c:\temp\demo" | --path "c:\temp\demo"`

    Runs migration from target directory.

 - `-c "<value>" | --connection-string "<value>"`

    Runs migration using the specified connection string.

 - `-a | --auto-create-db`

    Creates target database if the database does not exists.

 - `-t "v1.05" | --target-version "v1.05"`

    Runs migration only up to the version `v1.05` skipping `v1.06` or later.

 - `-k "<key>=<value>,<key>=<value>" | --token "<key>=<value>,<key>=<value>"`

    Replace each tokens in each script file. This is very helpful when you have environment specific sql-statements such as cross-server queries where database names are suffixed by the environment.

 - `--delimiter ";"`

    Runs bulk import of CSV files using `;` as delimiter.

 - `--platform "postgresql"`

    Runs migration targetting PostgreSql database.

 - `--command-timeout 120`
    
    The time in seconds to wait for the each command to execute.

 - `--environment "DEV"`

    Environment code for environment-aware scripts.

 - `-d | --debug`

    Runs migration with `DEBUG` tracing enabled.

 - `--help`

    Shows the CLI command reference on screen.

#### **`yuniql verify`**
---

Checks if all your versions can be executed without errors. It runs through all the non-versioned script folders (except `_init`) and all migration steps that `yuninql run` takes but without committing the transaction. All changes are rolled-back after a successful verification run.

>NOTE: Because it relies on an existing database, you can only use `verify` on database already baselined or versioned.

#### **`yuniql info`**
---

Shows all version currently present in the target database.

#### **`yuniql erase`**
---

Discovers and executes all scripts placed in the `_erase` directory. This is especially useful in dev and test environment where teams cannot auto-create new database each test case. The execution is immutable and enclosed in single transaction. The list of objects to drop, the order of when they will be dropped must be manually prepared. 

>WARNING: Be very careful when using this function as it has SEVERE consequences when run in PRODUCTION. Make sure to remove this pipeline task if you are cloning CI/CD pipelines from your DevOps tool.

#### **`yuniql version`**

Shows the current version of yuniql CLI running.

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
