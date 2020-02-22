+++
title = "Use Token Replacement"
description = "Replace tokens within scripts files. An alternative way to achieve environment-aware script execution."
bref = "Replace tokens within scripts files. An alternative way to achieve environment-aware script execution."
weight = 8
draft = false
toc = false
+++

A series of key/value pairs of tokens can be passed to `yuniql`. During migration run, yuniql inspects all script files and replaces them. This is particulary useful in cases such as cross-database and linked-server queries where the databases and server names varies per environment.

The following script would fail when run in TEST where `EMPLOYEEDB_DEV` database does not exists but `EMPLOYEEDB_TEST`.
```sql
SELECT E.FirstName, E.LastName, E.Address, E.Email 
FROM [EMPLOYEEDB_DEV].[dbo].[Employee] E 
ORDER BY E.FirstName ASC
```

To resolve this, let's pre-pare your script with token `%{ENV-DBNAME-SUFFIX}`. You can of course use whatever token name.
```sql
SELECT E.FirstName, E.LastName, E.Address, E.Email 
FROM EMPLOYEEDB_%{ENV-DBNAME-SUFFIX}.[dbo].[Employee] E 
ORDER BY E.FirstName ASC
```

Pass the tokens when you run migration

```shell
yuniql run -k "ENV-DBNAME-SUFFIX=DEV" -c "<you-dev-connection-string>"
yuniql run -k "ENV-DBNAME-SUFFIX=TEST" -c "<you-test-connection-string>"
yuniql run -k "ENV-DBNAME-SUFFIX=PROD" -c "<you-prod-connection-string>"
```

You may also pass the tokens as a series of key/value pairs separated by comma. 

```shell
yuniql run -k "token-key1=token-value1,token-key2=token-value2" -c "<you-dev-connection-string>"
yuniql run -k "token-key1=token-value1" -k "token-key2=token-value2" -c "<you-dev-connection-string>"
```

### Learn further

* [Migrate via ASP.NET Core]({{< ref "/docs/migrate-via-aspnetcore-application.md" >}})
* [Migrate via Azure DevOps]({{< ref "/docs/migrate-via-azure-devops-pipelines.md" >}})
* [Migrate via Docker Container]({{< ref "/docs/migrate-via-docker-container.md" >}})
* [Migrate via Console Application]({{< ref "/docs/migrate-via-netcore-console-application.md" >}})
* [Yuniql CLI Command Reference]({{< ref "/docs/yuniql-cli-command-reference.md" >}})
* [Bulk Import CSV Master Data]({{< ref "/docs/bulk-import-csv-master-data.md" >}})
* [Environment-aware Migration]({{< ref "/docs/environment-aware-scripts.md" >}})

#### Found bugs?

Help us improve further please [create an issue](https://github.com/rdagumampan/yuniql/issues/new).