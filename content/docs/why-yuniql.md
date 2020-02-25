+++
title = "Why yuniql"
description = "Pain points that yuniql solves and opportunities it presents to teams."
bref = "Pain points that yuniql solves and opportunities it presents to developers, data engineers and DBAs."
weight = 2
draft = false
toc = false
+++

#### Accelerate time to market
Developers wants control of the world, DBAs says they didn’t get the memo. In this dev and ops tug-of-war, the release gets delayed and opportunities are lost. Yuniql creates a Database DevOps model where developers and DBAs can co-own the schema evolution. Developers can also take full control of schema during development while DBAs review the Pull Requests to next version before transporting to Staging and Production.

#### Increase productivity
Increase velocity by empowering developers to manage and deploy local databases fast. With schema stored in shared repository, developers and data engineers can all work in parallel in creating database objects to support the features they are committed to. Each changes are continuously deployed via CI/CD and sends immediate feedback when any commit breaks the database or application. Yuniql and Docker lets developers fly high by automating local db deployment into database servers on Docker container.

#### Create change transparency
With every change in database becomes part of an atomic version, all modifications applied to the database are transparent. What has changed, who performed the change, when the change happens are captured in yuniql version tracking table and trace logs.

#### Prevent application downtime
An application code running on incompatible database schema will break. When it breaks, it may have already reached production and shouts out as major incident. Use yuniql API to get the current database version and match against expected version of the application or service.

#### Drive automated continuous delivery
While it maybe convenient to connect to database servers and execute critical change script, the risk for expensive or sometimes catastropic human errors is high! Keeps developers hands clean by automatic database deployment and change script execution via Release Pipelines. yuniql provides ready-to-use Azure DevOps Tasks, Docker base images and rich CLI commands to automate your deployment.

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).