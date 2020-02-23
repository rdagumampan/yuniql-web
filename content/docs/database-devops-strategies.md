+++
title = "Database DevOps Strategies"
description  = "Approaches to make development and database change management first-class citizens of your release process."
bref  = "Approaches to make development and database change management first-class citizens of your release process."
weight = 12
draft = true
toc = false
+++

**DRAFT, WORK IN PROGRESS**

Yuniql is a migration-based schema versioning and migration tool. Each migration step is a collection of change scripts and data placed in a version directory. There are also several utility directories to facilate pre and post migration tasks. A typical workflow starts with the developers/DBA creating an empty schema repository locally. 

```shell
md c:\temp\visitor-db
cd c:\temp\visitor-db

git init
Initialized empty Git repository in c:/temp/visitor-db/.git/
```

For here, we initialize the project directory with required directory structure. A directory `v0.00` is also created to represent the baseline version of your database.

```shell
yuniql init

INF   2020-02-21T04:52:24.7610599Z   Created script directory c:\temp\visitor-db\_init
INF   2020-02-21T04:52:24.7793155Z   Created script directory c:\temp\visitor-db\_pre
INF   2020-02-21T04:52:24.7925830Z   Created script directory c:\temp\visitor-db\v0.00
INF   2020-02-21T04:52:24.8077156Z   Created script directory c:\temp\visitor-db\_draft
INF   2020-02-21T04:52:24.8212725Z   Created script directory c:\temp\visitor-db\_post
INF   2020-02-21T04:52:24.8345398Z   Created script directory c:\temp\visitor-db\_erase
INF   2020-02-21T04:52:24.8514931Z   Created file c:\temp\visitor-db\README.md
INF   2020-02-21T04:52:24.8805998Z   Created file c:\temp\visitor-db\Dockerfile
INF   2020-02-21T04:52:24.9090545Z   Created file c:\temp\visitor-db\.gitignore
INF   2020-02-21T04:52:24.9244532Z   Initialized c:\temp\visitor-db.
```

Baseline your database by placing first batch of scripts in `v0.00`. If you are versioning an existing database, you can generate all the scripts required to re-create the database using your preferred tool. SSMS, SSDT, Sparx Enterprise Archiect, Visual Paradigm are some of popular enterprise tools for visual model-first database design. Assuming we are starting with new database, we create setup scripts.

```sql
CREATE TABLE [dbo].[Visitor](
	[VisitorID] [int] IDENTITY(1000,1) NOT NULL,
	[FirstName] [nvarchar](255) NULL,
	[LastName] [varchar](255) NULL,
	[Address] [nvarchar](255) NULL,
	[Email] [nvarchar](255) NULL
);
GO

...
... more scripts here
...
```

Database developers can follow these CLI sequence calls to prepare local db version before commiting to git repository:

- `yuniql init` / initializes db project structure
- `yuniql vnext` / increments version
- `yuniql run` / runs migrations
- `yuniql info` / shows existing versions applied
- `yuniql erase` / cleans up when done local testing

#### Learn further

* [Bulk Import CSV Master Data]({{< ref "/docs/bulk-import-csv-master-data.md" >}})
* [Use Token Replacement]({{< ref "/docs/token-replacement.md" >}})
* [Environment-aware Migration]({{< ref "/docs/environment-aware-scripts.md" >}})
* [Baseline Database]({{< ref "/docs/baseline-database.md" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).

<!-- ![](https://github.com/rdagumampan/yuniql/raw/master/assets/wiki-evodb-01.png)

![](https://github.com/rdagumampan/yuniql/raw/master/assets/wiki-how-it-works-dir.png)

>Image inspired by [Evolutionary Database Design](https://www.martinfowler.com/articles/evodb.html) by Martin Fowler and Pramod Sadalage. -->
<!-- 

