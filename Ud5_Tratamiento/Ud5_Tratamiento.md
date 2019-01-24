# Unidad didáctica 5. Tratamiento de los datos.

## Herramientas gráficas proporcionadas por el sistema gestor para la edición de la información.

### Introducción

Hay todo un mundo de aplicaciones visuales ([GUI](https://es.wikipedia.org/wiki/Interfaz_gráfica_de_usuario)) enfocadas en la administración y el uso de DBMS. Por poner un ejemplo, en este [post de un blog](http://www.palentino.es/blog/los-mejores-gestores-frontend-web-y-desktop-gui-para-mysql/) reunen un buen puñado y solo centrado en MySQL. Recordemos que existen más DBMS relacionales y no relacionales. Nosotros vamos a comentar brevemente un poco dos de ellas; phpMyAdmin y MySQLWorkbench, ambas son gratuitas.

### phpMyAdmin

*phpMyAdmin es un software gratuito basado en PHP destinado a la administración de servidores MySQL o MariaDB. Permite realizar la mayoría de las tareas de administración, incluido crear la base de datos, ejecutar las consultas o añadir usuarios.*

Esta herramienta está muy extendida puesto que forma parte de muchos kit ([XAMPP](https://www.apachefriends.org/es/index.html)) que proveen lo necesario para servidores web. Al ser web, es multiplataforma. Ubuntu la suele traer "de serie". Una de sus características distinguibles es la inserción de datos a través de formularios web.


[Documentación oficial, parcialmente en castellano](https://docs.phpmyadmin.net/es/latest/)
	
### MySQL Workbench

* También es multiplataforma: Windows, GNU/Linux y Mac.
* Permite desarollar diagramas E-R
* Es Software libre, distribuido bajo licencia GPL
* Permite crear script a partir del modelo creado y viceversa


### Microsoft Access

Si lo que se desea es usar el gestor Access para crear informes y formularios, manipular tablas, etc, pero sobre otro DBMS como MySQL, es posible mediante un conector [ODBC](https://es.wikipedia.org/wiki/Open_Database_Connectivity) TL:DR

*Open DataBase Connectivity es un estándar de acceso a las bases de datos desarrollado por SQL Access Group en 1992. El objetivo de ODBC es hacer posible el acceder a cualquier dato desde cualquier aplicación, sin importar qué sistema de gestión de bases de datos almacene los datos.*

[ODBC para MySQL](https://dev.mysql.com/downloads/connector/odbc/)

Una vez instalado, se puede crear un enlace desde el panel de control, herramientas administrativas, orígenes de datos.

Para el driver, hay que tener en cuenta la versión del sistema instalada (32 o 64 bits)

En la sección de *Datos externos / Más* habría que elegir *bases de datos de ODBC* y siguiendo el aistente es posible establecer un vínculo  al origen de datos y manejar la base de datos MySQL.

Esto aplica también a suites gratuitas como Openoffice.


## Inserción de registros. Inserción a partir de una consulta.

* Se basan en la sentencia ```INSERT INTO```. Los valores pueden ser cualquier constante, expresión o función o bien las palbaras clave *DEFAULT* o *NULL*.

* Se ha de especificar valor para todos los atributos de la lista de atributos y manteniendo el orden en el que aparecen.

* Si hay algún atributo de la tabla que no aparece en la lista, se le asigna el valor por defecto que tenga y si no tiene se deja a *null*.

* Si no se especifica la lista de atributos, se ha de dar valor a todos los atributos de la tabla en el orden especificado cuando se va a crear.


#### Un solo registro

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

#### Varios registros

```sql
INSERT INTO table_name(column1, column2, column3, ...)
(SELECT ....)
```

## Borrado de registros. Modificación de registros.

* Se basan en la sentencia ```DELETE```
* Elimina todos los registros de la tabla que cumplen la condición indicada
* Sólo se puede especificar una tabla

```sql
DELETE 
FROM table_name
WHERE column1 = value1;
```

* Se basan en la sentencia ```UPDATE```
* Modifican todos los registros de la tabla que cumplan la condición expresada
* Para cada registro, modifica el valor de todos los atributos que se indican, sustituyendo por el resultado de evaluar la expresión que se les asigna.
* Solo se puede expresar una tabla para actualizar
* La expresión puede ser cualquier constante, expresión o función, incluso una subconsulta que retorne un único valor

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

## Borrados y modificaciones e integridad referencial. Cambios en cascada.

* Todas las actualizaciones que provocan dejar de cumplir las reglas de integridad de la base de datos (la de entidades, referencial o definida por el usuario), producirán error y serán rechazadas.

* La eliminación o actualización de registros de una tabla puede provocar también la actualización o eliminación de registros de otras tablas relacionadas según las reglas de integridad referencial que estén definidas (CASCADE, SET NULL, etc.)


## Subconsultas y composiciones en órdenes de edición.
## Transacciones. Sentencias de procesamiento de transacciones.
## Problemas asociados al acceso simultáneo a los datos.
## Bloqueos compartidos y exclusivos. Políticas de bloqueo.
## Procesos y consultas masivas.