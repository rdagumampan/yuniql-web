+++
title = "Bulk Import CSV Master Data"
description = "Initiatlize lookup tables, load-up master data and create test samples during database deployment."
bref = "Initiatlize lookup tables, load-up master data and create test samples during database deployment."
weight = 355
draft = false
toc = false
+++

Master data and lookup tables almost comes natural as part of every database provisioning process. To support this, you may prepare a series of CSV files in the version directory. When you call `yuniql run`, yuniql will discovers the CSV files and bulk load into tables bearing same name as the CSV file. The following example demonstrates how to do this.

Install Yuniql CLI<br>
[{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}}).

```shell
choco install yuniql
```

Initialize local version

```shell
yuniql init
yuniql vnext
```

Create script file `setup_tables.sql` on `v0.01`.

```sql
CREATE TABLE Visitor (
	VisitorID INT IDENTITY(1000,1),
	FirstName NVARCHAR(255),
	LastName VARCHAR(255),
	Address NVARCHAR(255),
	Email NVARCHAR(255)
);
```

Create a `Visitor.csv` on version `v0.01`. If your target table has an schema, you can name the file with schem name such as `Registration.Visitor` where Registration is a schema.

```csv
"VisitorID","FirstName","LastName","Address","Email"
"1000","Jack","Poole","Manila","jack.poole@never-exists.com"
"1001","Diana","Churchill","Makati","diana.churchill@never-exists.com"
"1002","Rebecca","Lyman","Rizal","rebecca.lyman@never-exists.com"
"1003","Sam","Macdonald","Batangas","sam.macdonald@never-exists.com"
"1004","Matt","Paige","Laguna","matt.paige@never-exists.com"
```

>NOTE: The file name of the CSV file must match the destination table else an exception is thrown.

Run migration

```shell
yuniql run -a -c "<your-connection-string>"

INF   2019-10-22T18:36:08.7621330Z   Executed script file C:\temp\yuniql-nightly\v0.01\setup-tables.sql.
INF   2019-10-22T18:36:08.7638901Z   Found the 1 csv files on C:\temp\yuniql-nightly\v0.01
INF   2019-10-22T18:36:08.7649367Z   Visitor.csv
INF   2019-10-22T18:36:09.0854032Z   Imported csv file C:\temp\yuniql-nightly\v0.01\Visitor.csv.
```

Verify if all is good

```sql
SELECT * FROM [dbo].[Visitor]
```

A working sample is available here for your reference
https://github.com/rdagumampan/yuniql/tree/master/samples/sqlserver-all-features-sample/v0.00

#### Learn further

* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})
* [Use Token Replacement]({{< ref "/docs/token-replacement.md" >}})
* [Environment-aware Migration]({{< ref "/docs/environment-aware-scripts.md" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).