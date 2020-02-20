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
