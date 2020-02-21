+++
title = "How Yuniql Works"
description = "Understand the design principles, ways of working and internals of yuniql."
bref = "yuniql is data platform devops tool. Schema versioning and database migration is one of its core capabilities. Understand its internals here."
weight = 10
draft = false
toc = false
+++

**DRAFT**

Yuniql organizes the database migration steps in series of directories and files. Each directory represents an atomic version of your database executed in an all-or-nothing fashion. Utility directories are also created to cover pre and post migration activities. A typical starter project workspace looks like this.

```shell
C:\temp\sqlserver-sample>dir

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
11/10/2019  10:43    <DIR>          _pr.
```

When `yuniql run` is issued the first time, it inspects the target database and creates required table to track the versions. All script files in `_init` directory will be executed only this time. The order of execution is as follows `_init`, `_pre`, `vx.xx`, `vxx.xx+N`, `_draft` , `_post`. A successful first-run would have tracking the table filled-up this way.

```sql
SELECT * FROM __YuniqlDbVersion;

SequenceId	Version	AppliedOnUtc	        AppliedByUser	            AppliedByTool	AppliedByToolVersion
1	        v0.00	2020-02-21 06:17:08.147	DESKTOP-ULR8GDO\rdagumampan	yuniql-cli	    v0.350.0.0	NULL
```

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
