+++
title = "Quick Start"
description = "Up and running in under 10 minutes"
weight = 10
draft = false
toc = false
bref = "This is an express guide to using yuniql-cli. Yuniql allows developers and DBAs to run migration steps from CLI. Run these commands line by line via Command Prompt (CMD)."
+++

#### Install yuniql
Install yuniql CLI with Chocolatey or use alternative ways listed here https://github.com/rdagumampan/yuniql/wiki/Install-yuniql

```console
choco install yuniql --version 0.350.0
```

#### Download samples
Clone yuniql repo and play with several samples for sqlserver, postgresql, mysql and other platforms. Find your samples here https://github.com/rdagumampan/yuniql/tree/master/samples
```console
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql-getstarted
cd c:\temp\yuniql-getstarted\samples\sqlserver-all-features-sample
```

#### Prepare connection
Set your db connection string in environment variable. This demo uses local SQL Server instance. For more connection string samples, visit https://www.connectionstrings.com/sql-server/.

```console
SETX YUNIQL_CONNECTION_STRING "Server=.\;Database=helloyuniql;Trusted_Connection=True;"
```

#### Run migration<br>
The following commands `yuniql` to discover the project directory, creates the target database if it doesn't exist and runs all migration steps in the order they are listed. These includes `.sql` files, directories, subdirectories, and csv files. Tokens are also replaced via `-k` parameters.
```console
cd c:\temp\yuniql-getstarted\samples\sqlserver-all-features-sample

yuniql run -a -k "VwColumnPrefix1=Vw1,VwColumnPrefix2=Vw2,VwColumnPrefix3=Vw3,VwColumnPrefix4=Vw4"
yuniql info

Version         Created                         CreatedBy
v0.00           2019-11-03T16:29:36.0130000     DESKTOP-ULR8GDO\rdagumampan
v1.00           2019-11-03T16:29:36.0600000     DESKTOP-ULR8GDO\rdagumampan
v1.01           2019-11-03T16:29:36.1130000     DESKTOP-ULR8GDO\rdagumampan
```

#### Verify results<br>
Query tables with SSMS or your preferred SQL client
```
//SELECT * FROM [dbo].[Visitor]
VisitorID   FirstName   LastName    Address  Email
----------- ----------- ----------- ------------------------------------------
1000        Jack        Poole       Manila   jack.poole@never-exists.com
1001        Diana       Churchill   Makati   diana.churchill@never-exists.com
1002        Rebecca     Lyman       Rizal    rebecca.lyman@never-exists.com
1003        Sam         Macdonald   Batangas sam.macdonald@never-exists.com
1004        Matt        Paige       Laguna   matt.paige@never-exists.com
```

<br>
<img align="center" src="https://github.com/rdagumampan/yuniql/raw/master/assets/visitordb-screensot-ssms.png" width="700">

### Further readings

* [Migrate via ASP.NET Core](https://github.com/rdagumampan/yuniql/wiki/How-to-run-migration-from-ASP.NET-Core)
* [Migrate via Azure DevOps](https://github.com/rdagumampan/yuniql/wiki/How-to-run-migration-from-Azure-Devops)
* [Migrate via Docker](https://github.com/rdagumampan/yuniql/wiki/How-to-run-migration-from-docker-container)

### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
