+++
title = "How Yuniql Works"
description = "yuniql is data platform devops tool. Understand the design principles and internals here."
bref = "yuniql is data platform devops tool. Schema versioning and database migration is one of its core capabilities. Understand its internals here."
weight = 10
draft = false
toc = false
+++

Yuniql organizes database schema version using series of ordinary directories and SQL files. Several utility directories and files are also included in default structure to support pre and post migration tasks. A typical baseline structure is described below and migration follows this order `_init [only on first-run]`,`_pre`,`v0.00`,`v.x.xx`,`vx.xx+1`,`_draft`,`_post`.

<img src="https://github.com/rdagumampan/yuniql/raw/master/assets/wiki-how-it-works-dir.png" width=700>

#### Typical workflow
Developers and DBAs can work in these sequence:
- `yuniql init` / initializes db project structure
- `yuniql vnext` / increment version
- `yuniql run` / run migrations
- `yuniql info` / show existing versions

#### Supported CLI Commands
##### `yuniql init`
When `yuniql init` is issued, a baseline directory structure will be created automatically.

| Directory / File | Usage Description | Execution |
| --- | --- | --- |
| *_init* | Initialization scripts. <br>Executed the first time `yuniql run` is issued|Executed once |
| *_pre* | Pre migration scripts. <br>Executed every time before any version. | Every migration run |
| *v0.00* | Baseline scripts. <br>Executed when `yuniql run`. | Executed once |
| *_draft* | Scripts in progress. <br>Scripts that you are currently working <br>and have not moved to specific version directory yet. <br>Executed every time after the latest version. | Every migration run |
| *_post* | Post migration scripts. <br>Executed every time and always the last batch to run. | Every migration run |
| *_erase* | Database cleanup scripts. <br>Executed once only when `yuniql erase` is issued. | Executed on demand |
| *Dockerfile* | A template docker file to run your migration. <br>Uses docker base images with `yuniql` installed.| Executed on `docker build` |
| *README.md* | A template README file.| |
| *.gitignore* | A template git ignore file to skip yuniql.exe from being committed.| |

##### `yuniql vnext`
When `yuniql vnext` is issued, it identifies the latest version locally and increment the minor version with the format `v{major}.{minor}`. The command just helps reduce human errors but this can also be done manually.

| `yuniql vnext` CLI Arguments |
| :--- |
|`-m`<br>`--minor`<br>Default option. Increments major version by creating `vx.xx+1` folder|
|`-M`<br>`--major`<br>Increments minor version by creating `vx+1.xx` folder|
|`-f`<br>`--file <file-name.sql>`<br>Creates an empty sql file in the created major or minor version|

##### `yuniql run`
When `yuniql run` is issued the first time, it inspects the target database and creates required table to track the versions. All script files in `_init` directory will be executed. The order of execution is as follows `_init`,`_pre`,`vx.xx`,`_draft`,`_post`. Several variations on how we can run migration are listed below.

| `yuniql run` CLI Arguments |
| :--- |
| `-a`<br>`--auto-create-db`<br>Runs migration using connection string from environment variable `YUNIQL_CONNECTION_STRING`<br>Auto-create target database if not exists|
| `-c "<value>"`<br>`--connection-string "<value>"`<br>Runs migration using the specified connection string|
| `-p c:\temp\demo`<br>`--path c:\temp\demo`<br>Runs migration from target directory |
| `-t v1.05`<br>`--target-version v1.05`<br>Runs migration only up to the version `v1.05` skipping `v1.06` or later|
| `-k "<key>=<value>,<key>=<value>"`<br>`--token "<key>=<value>,<key>=<value>"`<br>Replace each tokens in each script file. This is very helful when you have environment specific sql-statements such as cross-server queries where database names are suffixed by the environment.|
| `--delimiter ";"`<br>Runs migration using `;` as CSV file delimiter |
| `-d`<br>`--debug`<br>Runs migration with `DEBUG` tracing enabled|
| `--platform "postgresql"`<br>Runs migration in PostgreSql database.|

##### `yuniql verify`

When `yuniql verify` is issued, it checks if all your versions can be executed without errors. It runs through all the non-versioned script folders (except `_init`) and all migration steps that `yuninql run` takes but without committing the transaction. All changes are rolled-back after a successful verification run.

>NOTE: Because it relies on an existing database, you can only use `verify` on database already baselined or versioned.

##### `yuniql info`

`yuniql info` shows all version currently present in the target database.

##### `yuniql erase`

`yuniql erase` discovers and executes all scripts placed in the `_erase` directory. This is especially useful in dev and test environment where teams cannot auto-create new database each test case. The execution is immutable and enclosed in single transaction. The list of objects to drop, the order of when they will be dropped must be manually prepared. 

>WARNING: Be very careful when using this function as it has SEVERE consequences when run in PRODUCTION. Make sure to remove this pipeline task if you are cloning CI/CD pipelines from your DevOps tool.

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
