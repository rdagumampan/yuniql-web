+++
title = "Yuniql CLI Command Reference"
description = "Rich CLI commands and parameters for running migrations locally and on continuous integration server."
bref = "Command Line Interface (CLI) commands and parameters."
weight = 11
draft = false
toc = false
+++

Yuniql CLI is powerful interface to prepare and run migrations from developer's IDE, DBA's machine or thru continuous integration server. Database developers can follow these CLI sequence calls to prepare local db version before commiting to git repository:

- `yuniql init` / initializes db project structure
- `yuniql vnext` / increments version
- `yuniql run` / runs migrations
- `yuniql info` / shows existing versions applied
- `yuniql erase` / cleans up when done local testing

##### **`yuniql init`**
---
Creates baseline directory structure that serves as your database migration workspace. Commit this into your preferred source control platform such as `git`, `tfs vc` or `svn`. 
```shell
yuniql init [-p|--path] [-d|--debug] [--help]
```

- `-p "c:\temp\demo" | --path "c:\temp\demo"`

    Creates initial directory structure in the target directory. Defaults to current directory of the CLI.

- `-d | --debug`

    Runs command with `DEBUG` tracing enabled.

- `--help`

    Shows the CLI command reference on screen.

##### **`yuniql vnext`**
---
Identifies the latest version locally and increments the minor version with the format `v{major}.{minor}`. The command just helps reduce human errors and this can also be done manually.

```shell
yuniql vnext [-p|--path] [-M|--major] [-m|--minor] [-f|--file] [-d|--debug] 
```

- `-p "c:\temp\demo" | --path "c:\temp\demo"`

    Creates new version in the target directory. Defaults to current directory of the CLI.

- `-m | --minor`

    Increments major version by creating `vx.xx+1` folder. Default when not passed.

- `-M | --major`

    Increments minor version by creating `vx+1.xx` folder.

- `-f | --file <file-name.sql>`

    Creates an empty sql file in the created major or minor version.

- `-d | --debug`

    Runs command with `DEBUG` tracing enabled.

- `--help`

    Shows the CLI command reference on screen.

##### **`yuniql run`**
---
Inspects the target database and creates required table to track the versions. All script files in `_init` directory will be executed. The order of execution is as follows `_init`,`_pre`,`vx.xx`,`_draft`,`_post`. Several variations on how we can run migration are listed below.

```shell
yuniql run [-p|--path] [-c|--connection-string] [-a|--auto-create-db] [-t|--target-version] 
    [-k|--token] [--delimeter] [--platform] [--command-timeout] [--environment] [-d|--debug]
```

- `-p "c:\temp\demo" | --path "c:\temp\demo"`

    Runs migration from target directory. Defaults to current directory of the CLI.

- `-c "<value>" | --connection-string "<value>"`

    Runs migration using the specified connection string. Defaults to environment variable `YUNIQL_CONNECTION_STRING`. See environment variables.

- `-a | --auto-create-db`. Defaults to `false`.

    Creates target database if the database does not exists. Defaults to `false`.

- `-t "v1.05" | --target-version "v1.05"`

    Runs migration only up to the version `v1.05` skipping `v1.06` or later. Defaults to latest available version locally.

- `-k "<key>=<value>,<key>=<value>" | --token "<key>=<value>,<key>=<value>"`

    Replace each tokens in each script file. This is very helpful when you have environment specific sql-statements such as cross-server queries where database names are suffixed by the environment.

- `--delimiter ";"`

    Runs bulk import of CSV files using `;` as delimiter. Defaults to `;`;

- `--platform "postgresql"`

    Runs migration targetting PostgreSql database. Defaults to `sqlserver`. See supported platforms.

- `--command-timeout 120`
    
    The time in seconds to wait for the each command to execute. Defaults to 30 secs.

- `--environment "DEV"`

    Environment code for environment-aware scripts.

- `-d | --debug`

    Runs command with `DEBUG` tracing enabled. Prints raw sql statements before their execution.

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

- `-p "c:\temp\demo" | --path "c:\temp\demo"`

    Runs command from target directory. Defaults to current directory of the CLI.

- `-c "<value>" | --connection-string "<value>"`

    Runs command using the specified connection string. Defaults to environment variable `YUNIQL_CONNECTION_STRING`. See environment variables.

- `--platform "postgresql"`

    Runs command targetting `PostgreSql` database. Defaults to `sqlserver`. See supported platforms.

- `--command-timeout 120`
    
    The time in seconds to wait for the each command to execute. Defaults to 30 secs.

- `-d | --debug`

    Runs command with `DEBUG` tracing enabled. Prints raw sql statements before their execution.

- `--help`

    Shows the CLI command reference on screen.

##### **`yuniql erase`**
---
Discovers and executes all scripts placed in the `_erase` directory. This is especially useful in dev and test environment where teams cannot auto-create new database each test case. The execution is immutable and enclosed in single transaction. The list of objects to drop, the order of when they will be dropped must be manually prepared. 

> NOTE: Yuniql does not automatically create drop scripts but instead rely on users to prepare the appropriate way of clearing the database objects. With this, you can segment environment-specific clean-up scripts; example is when you may not want to drop permissions in DB when you are performing integration tests.

```shell
yuniql erase [-p|--path] [-c|--connection-string]  [-k|--token] [--platform] [--command-timeout] [--force] [-d|--debug]
```

- `-p "c:\temp\demo" | --path "c:\temp\demo"`

    Runs command from target directory. Defaults to current directory of the CLI.

- `-c "<value>" | --connection-string "<value>"`

    Runs command using the specified connection string. Defaults to environment variable `YUNIQL_CONNECTION_STRING`. See environment variables.

- `--platform "postgresql"`

    Runs command targetting `PostgreSql` database. Defaults to `sqlserver`. See supported platforms.

- `--command-timeout 120`
    
    The time in seconds to wait for the each command to execute. Defaults to 30 secs.

- `--force`

    Runs erase command without asking for confirmation. [Not yet implemented].

- `-d | --debug`

    Runs command with `DEBUG` tracing enabled. Prints raw sql statements before their execution.

- `--help`

>WARNING: Be very careful when using this function as it has SEVERE consequences when run in PRODUCTION. Make sure to remove this pipeline task if you are cloning CI/CD pipelines from your DevOps tool.

##### **`yuniql version`**

Shows the current version of yuniql CLI running.

```shell
yuniql version
```

### Learn further

* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
