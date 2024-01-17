---
title: "How to set a SQL database in Microsoft SQL Server to offline"
date: "2013-04-29"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "mssql"
  - "sql"
  - "sysadmin"
  - "technology"
---

After moving a database to a new server (via backup and restore utilities built into Microsoft SQL Server) I wanted to take the copy on the old server offline to ensure when testing the move I was only accessing the new database but due to my limited (but growing) knowledge I had no idea how to take a database offline.

The Management Studio application gives you the option to dismount the database but not set it to offline. To set a database to offline, open a new SQL query window and use the following SQL query:

```sql
ALTER DATABASE [databasename] SET OFFLINE WITH
ROLLBACK AFTER 30 SECONDS
```

Just replace ```databasename``` with the name of the database you would like to set to offline. This will set the database to offline after 30 seconds (to allow any transactions to close gracefully). Alternatively, if you are impatient we can kill any transactions and connections to the database immediately and set it to offline with the following:

```sql
ALTER DATABASE [databasename] SET OFFLINE WITH
ROLLBACK IMMEDIATE
```

Another skill for your sysadmin toolkit!
