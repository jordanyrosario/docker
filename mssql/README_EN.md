# Overview

- Build a docker image based on `mcr.microsoft.com/mssql/server:2019-CU13-ubuntu-20.04`
- Configure the database with a database and user

# How to Run


## Running the container

Modify the env variables to your liking in the `docker-compose.yml`.

Then spin up a new container using `docker-compose`

```

docker-compose up

```

Note: MSSQL passwords must be at least 8 characters long, contain upper case, lower case and digits.
Configuration of the server will occur once it runs; the MSSQL\* env variables are required for this step.

Note: add a `-d` to run the container in background

## Connecting to the container

To connect to the SQL Server in the container, you can docker exec with sqlcmd.

```

docker exec -it mssqldev /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P $SA_PASSWORD

```

The next command uses the SQL Server command line utility sqlcmd to execute some SQL commands contained in the newly created param_setup file.

```

/opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P Your_SA_Password -d master -i param_setup.sql

```

The setup.sql script will create a new database based on the env variable `$MSSQL_DB` and a user based on`$MSSQL_USER` with password `$MSSQL_PASSWORD` in the default `dbo` schema.

## setup.sql

The setup.sql defines SQL commands to create a database along with a user login with admin permissions.

```

CREATE DATABASE $(MSSQL_DB);
GO
USE $(MSSQL_DB);
GO
CREATE LOGIN $(MSSQL_USER) WITH PASSWORD = '$(MSSQL_PASSWORD)';
GO
CREATE USER $(MSSQL_USER) FOR LOGIN $(MSSQL_USER);
GO
ALTER  ROLE db_owner ADD MEMBER [$(MSSQL_USER)];
GO


```

```
