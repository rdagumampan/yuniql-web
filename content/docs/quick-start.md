+++
title = "Quick Start"
description = "Up and running in under 10 minutes"
weight = 10
draft = false
toc = false
bref = "YUNIQL simply automate what you would normally by hand. Starting up a database migration takes few minutes, here's how to get started from zero"
+++

## To start using **`yuniql`**

1. Clone sample project
	```bash
	git clone https://github.com/rdagumampan/yuniql c:\temp\yuniql
	cd c:\temp\yuniql\sqlserver-samples\visitph-db
	```

2. Download latest `yuniql` build<br>

	```bash
	powershell Invoke-WebRequest -Uri https://ci.appveyor.com/api/projects/rdagumampan/yuniql/artifacts/yuniql-nightly.zip -OutFile  "c:\temp\yuniql\yuniql-nightly.zip"
	powershell Expand-Archive "c:\temp\yuniql\yuniql-nightly.zip" -DestinationPath "c:\temp\yuniql\sqlserver-samples\visitph-db"
	```

	>`Expand-Archive` requires at least powershell v5.0+ running on your machine. You may also [download manually here](https://ci.appveyor.com/api/projects/rdagumampan/yuniql/artifacts/yuniql-nightly.zip) and extract to desired directory.

3. Set default connection string to target database<br>
	- Using an sql account<br>
	`Server=<server-instance>,[<port-number>];Database=VisitorDB;User Id=<sql-user-name>;Password=<sql-user-password>`	
	- Using trusted connection<br>
	`Server=<server-instance>,[<port-number>];Database=VisitorDB;Trusted_Connection=True;`<br><br>

	```bash
	SETX YUNIQL_CONNECTION_STRING "Server=.\;Database=VisitorDB;Trusted_Connection=True;"
	```

4. Run migration<br>
The following commands `yuniql` to discover the project directory, creates the target database if it doesn't exist and runs all migration steps in the order they are listed. These includes `.sql` files, directories, subdirectories, and csv files. Tokens are also replaced via `-k` parameters.
	```bash
	yuniql run -a -k "VwColumnPrefix1=Vw1,VwColumnPrefix2=Vw2,VwColumnPrefix3=Vw3,VwColumnPrefix4=Vw4"
	yuniql info

	Version         Created                         CreatedBy
	v0.00           2019-11-03T16:29:36.0130000     DESKTOP-ULR8GDO\rdagumampan
	v1.00           2019-11-03T16:29:36.0600000     DESKTOP-ULR8GDO\rdagumampan
	v1.01           2019-11-03T16:29:36.1130000     DESKTOP-ULR8GDO\rdagumampan
	```

5. Verify results<br>
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
	<img align="center" src="/images/visitordb-screensot-ssms.png" width="700">
