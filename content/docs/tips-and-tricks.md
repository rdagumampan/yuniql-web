+++
title = "Tips and Tricks"
description = "Increase productivity, learnings from experience of others, known limitations and work around."
bref = "Increase productivity, learnings from experience of others, known limitations and work around."
weight = 10
draft = false
toc = false
+++

#### Get some sanity, organize your project into sub-directories

When versioning an existing database, you may end creating a giant script file with all the noise created by your script-generation tool. As yuniql supports child directories, you may organize your version directory as demonstrated below. Use a sequence number prefix to guarantee they executed in the right order.

```shell
cd C:\play\yuniql\samples\sqlserver-adventureworkslt2016-sample
cd v0.00
dir

Directory of C:\play\yuniql\samples\sqlserver-adventureworkslt2016-sample\v0.00
11/03/2019  13:06    <DIR>          01-schemas
11/03/2019  13:06    <DIR>          02-types
11/03/2019  13:06    <DIR>          03-xmlschemas
11/03/2019  16:43    <DIR>          04-tables
11/03/2019  16:51    <DIR>          08-sequences
11/03/2019  16:51    <DIR>          09-triggers
11/10/2019  10:40                98 README.md
```

```shell
cd 04-tables
dir 

Directory of C:\play\yuniql\samples\sqlserver-adventureworkslt2016-sample\v0.00\04-tables
10/29/2019  06:32             5,696 003-SalesLT.Address.sql
10/29/2019  06:32             7,542 004-SalesLT.Customer.sql
10/29/2019  06:32             4,726 005-SalesLT.CustomerAddress.sql
10/29/2019  06:32             5,042 006-SalesLT.ProductCategory.sql
10/29/2019  06:32             3,072 007-SalesLT.ProductModel.sql
10/29/2019  06:32            10,844 008-SalesLT.Product.sql
10/29/2019  06:32             3,528 009-SalesLT.ProductDescription.sql
10/29/2019  06:32             5,165 010-SalesLT.ProductModelProductDescription.sql
10/29/2019  06:32            19,684 011-SalesLT.SalesOrderHeader.sql
10/29/2019  06:32            10,193 012-SalesLT.SalesOrderDetail.sql
```

A sample is available here for reference https://github.com/rdagumampan/yuniql/tree/master/samples/sqlserver-adventureworkslt2016-sample

#### Make your application compatible with the DB again

When you need to ensure that the application always runs to a compatible schema version of the database, you may use Yuniql API to assert this when your service runs. An example is described below. 


```csharp
using Yuniql.Core;

...
var requiredDbVersion = "v1.01";
var currentDbVersion = migrationService.GetCurrentVersion();
if(currentDbVersion != requiredDbVersion)
{
    throw new ApplicationException($"Startup failed. " +
        $"Application requires database version {requiredDbVersion} but current version is {currentDbVersion}." +
        $"Deploy the latest compatible schema version of database and run again.");
}
...
```
A sample is available here for reference https://github.com/rdagumampan/yuniql/tree/master/samples/sqlserver-console-sample

#### You don't like Comma in CSV, try with Pipe |

Using CSV files is an effective way to bulk load your master data and look-up table. It looks nice but you don't like Comma... Maybe its better with Semi-colon ";" or Pipe "|". Try this way

```csharp
using Yuniql.AspNetCore;


app.UseYuniql(traceService, new YuniqlConfiguration
{
    WorkspacePath = Path.Combine(Environment.CurrentDirectory, "_db"),
    ConnectionString = "Server=localhost,1400;Database=yuniqldb;User Id=SA;Password=P@ssw0rd!",
    AutoCreateDatabase = true,
    Delimiter = "|",
});
```

When you prefer to run migrations with Yuniql CLI.

```shell
yuniql run --delimiter "|"
```

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).