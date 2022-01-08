+++
title = "Why yuniql"
description = "Pain points that yuniql solves and opportunities it presents to teams."
bref = "Accelerate delivery. Release database changes safely. Challenges that yuniql solves and opportunities it presents to developers, data engineers and DBAs."
weight = 404
draft = false
toc = false
+++

#### Accelerates time to market
Developers wants control of the world, DBAs says they didn’t get the memo. In this dev and ops tug-of-war, the release gets delayed and opportunities are lost. yuniql promotes a Database DevOps model where developers and DBAs can co-own the schema evolution. A picture where developers can also take full control of schema during development while DBAs review the Pull Requests to next version before transporting to Staging and Production.

#### Increases productivity
Increase velocity thru greater autonomy of developers to manage and deploy local databases fast. With schema stored in shared repository, developers and data engineers can all work in parallel in creating database objects to support the features they are committed to. Each changes are continuously deployed via CI/CD and feedback is sent immediately when commit breaks the integration. With Docker, yuniql also enables developers automate local db deployment into containerized database servers.

#### Creates change transparency
With every change in database becomes part of an atomic version, all modifications applied to the database are transparent. What has changed, who performed the change, when the change happens are captured in yuniql version tracking table and trace logs.

#### Prevents application downtime
An application code running on incompatible database schema will break. When it breaks, it may have already reached production and shouts out as major incident. Developers can use yuniql API to get the current database version and match against expected version of the application.

#### Drives automation & continuous delivery
While it maybe convenient to connect to database servers and execute critical change script, the risk for expensive or sometimes catastropic human errors is high! Keeps developers hands clean by automatic database deployment and change script execution exclusively thru devops pipelines. yuniql provides ready-to-use Azure DevOps Tasks, Docker base images and rich CLI commands to automate deployment of database changes.

#### Learn further

* [How yuniql works]({{< ref "/docs/how-yuniql-works.md" >}})
* [Get started with Sql Server]({{< ref "/docs/get-started.md" >}})
* [Get started with PostgreSql, MySql and others]({{< ref "/docs/get-started-postgresql.md" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).