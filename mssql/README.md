# Descripcion

- Crea un contenedor basado en la imagen  `mcr.microsoft.com/mssql/server:2019-CU13-ubuntu-20.04` de SQLServer
- Configura la instacia con una base de datos y un usuario

# Como ejecutar


## Lanzar el contenedor

Modifique las variables de entorno de el archivo `docker-compose.yml`.

Lance el contenedor  con `docker-compose`

```

docker-compose up

```

Nota: La contrasena de MSSQL debe ser por lo menos 8 caracteres, contener mayusculas, minusculas y digitos.
La configuracion de MSSQL ocurrira cuando entre en ejecucion; las variables de entorno  MSSQL\*  son obligatorias para este paso.

Nota: agrege `-d` para que la instacia corra en el background.

## Coneccion al contenedor 

Para conectar al contenedor SQL Server puede usar `docker exec` con el comando `sqlcmd`.

```

docker exec -it mssqldev /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P $SA_PASSWORD

```

El script `setup.sql` crea una nueva base de datos usando los valores presentes en las variables de entorno `$MSSQL_DB` para la base de datos , `$MSSQL_USER` para el usuario y `$MSSQL_PASSWORD` para la contrasena, por defecto usando el schema `dbo`.

## setup.sql

The setup.sql defines SQL commands to create a database along with a user login with admin permissions.
`setup.sql` define los comandos para crear la base de datos, asi como, crear el login, usuario, y asignar los privilegios correspondientes.

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
