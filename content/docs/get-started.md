+++
title = "Get started!"
description = "Kick-start your database devops with yuniql CLI. Done in 10 mins max."
bref = "This is an express guide to using yuniql CLI. Yuniql allows developers and DBAs to run migration steps from CLI. Run these commands line by line via Command Prompt (CMD)."
weight = 2
draft = false
toc = false
+++

#### Install yuniql
Install yuniql CLI with Chocolatey or use alternative ways listed here  [{{< ref "/docs/install-yuniql.md" >}}]({{< ref "/docs/install-yuniql.md" >}})

```shell
choco install yuniql
```

#### Download samples
Clone yuniql repo and play with several samples for sqlserver, postgresql, mysql and other platforms. Find your samples here https://github.com/rdagumampan/yuniql/tree/master/samples
```shell
git clone https://github.com/rdagumampan/yuniql.git c:\temp\yuniql-getstarted
cd c:\temp\yuniql-getstarted\samples\sqlserver-all-features-sample
```

#### Prepare connection
Set your db connection string in environment variable. This demo uses local SQL Server instance. For more connection string samples, visit https://www.connectionstrings.com/sql-server/.

```shell
SETX YUNIQL_CONNECTION_STRING "Server=.\;Database=helloyuniql;Trusted_Connection=True;"
```

#### Run migration<br>
The following commands `yuniql` to discover the project directory, creates the target database if it doesn't exist and runs all migration steps in the order they are listed. These includes `.sql` files, directories, subdirectories, and csv files. Tokens are also replaced via `-k` parameters.
```shell
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
```sql
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

### Learn further

* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})

### Found bugs?
Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).
