+++
title = "Yuniql CLI Command Reference"
description = "User Yuniql rich CLI commands and parameters for running migrations locally and on server."
bref = "Command Line Interface (CLI) commands and parameters."
weight = 10
draft = false
toc = false
+++

##### **`yuniql init`**
---
Creates baseline directory structure that serves as your database migration workspace. Commit this into your preferred source control platform such as `git`, `tfs vc` or `svn`. 
```shell
yuniql init [-p|--path] [-d|--debug]
```

##### **`yuniql vnext`**
---
Identifies the latest version locally and increment the minor version with the format `v{major}.{minor}`. The command just helps reduce human errors and this can also be done manually.

```shell
yuniql vnext [-p|--path] [-M|--major] [-m|--minor] [-f|--file] [-d|--debug] 
```

- `-m | --minor`

    Default option. Increments major version by creating `vx.xx+1` folder

- `-M | --major`

    Increments minor version by creating `vx+1.xx` folder

- `-f | --file <file-name.sql>`

    Creates an empty sql file in the created major or minor version

##### **`yuniql run`**
---
Inspects the target database and creates required table to track the versions. All script files in `_init` directory will be executed. The order of execution is as follows `_init`,`_pre`,`vx.xx`,`_draft`,`_post`. Several variations on how we can run migration are listed below.

```shell
yuniql run [-p|--path] [-c|--connection-string] [-a|--auto-create-db] [-t|--target-version] 
    [-k|--token] [--delimeter] [--platform] [--command-timeout] [--environment] [-d|--debug]
```

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

##### **`yuniql verify`**
---
Checks if all your versions can be executed without errors. It runs through all the non-versioned script folders (except `_init`) and all migration steps that `yuninql run` takes but without committing the transaction. All changes are rolled-back after a successful verification run.

```shell
yuniql verify [-p|--path] [-c|--connection-string] [-t|--target-version] 
    [-k|--token] [--delimeter] [--platform] [--command-timeout] [--environment] [-d|--debug]
```

>NOTE: Because it relies on an existing database, you can only use `verify` on database already baselined or versioned.

##### **`yuniql info`**
---
Shows all version currently present in the target database.

```shell
yuniql info [-p|--path] [-c|--connection-string] [--platform] [--command-timeout] [-d|--debug]
```

##### **`yuniql erase`**
---
Discovers and executes all scripts placed in the `_erase` directory. This is especially useful in dev and test environment where teams cannot auto-create new database each test case. The execution is immutable and enclosed in single transaction. The list of objects to drop, the order of when they will be dropped must be manually prepared. 

```shell
yuniql erase [-p|--path] [-c|--connection-string]  [-k|--token] [--force] [--platform] [--command-timeout] [-d|--debug]
```

>WARNING: Be very careful when using this function as it has SEVERE consequences when run in PRODUCTION. Make sure to remove this pipeline task if you are cloning CI/CD pipelines from your DevOps tool.

##### **`yuniql version`**

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
