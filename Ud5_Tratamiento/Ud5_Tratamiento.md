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

* Si no se especifica la lista de atributos, se ha de dar valor a todos los atributos de la tabla en el orden especificado cuando se creó la tabla.

* Si se intenta insertar más valores que columnas tenga la tabla, se producirá un error.


#### Un solo registro

```sql
# Algunas columnas
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);

# Todas columnas
INSERT INTO table_name 
VALUES (value1, value2, value3)

# Columnas con valores por defecto
INSERT INTO table_name
VALUES (value1, DEFAULT, value3)
```

#### Varios registros

* Es posible insertar más de un registro en un solo ```ÌNSERT INTO```.

```sql
INSERT INTO jugadores(nombre, apellido, posicion)
VALUES 	('Pau', 'Gasol', 'Pivot'),
		('Luka','Dončić','Alero'),
		('Stephen','Curry','Base');
```
* También es posible insertar en una tabla el resultado de una consulta. En este caso, la consulta debe devolver exactamente las columnas a insertar y puede ser tan compleja como se desee a nivel de filtros, agrupamientos, ordenación, etc.

```sql
INSERT INTO table_name [(column1, column2, column3, ...)]
	(SELECT ...
		FROM ... )
```

## Modificación de registros.

* Se basan en la sentencia ```UPDATE```

* Modifican todos los registros de la tabla que cumplan la condición expresada.

* Para cada registro, modifica el valor de todos los atributos que se indican, sustituyendo por el resultado de evaluar la expresión que se les asigna.

* Solo se puede expresar una tabla para actualizar.

* Si no se expresa filtro ```WHERE``` se modifican todas las filas de la tabla.

* La expresión puede ser cualquier constante, expresión o función, incluso una subconsulta que retorne un único valor

```sql
UPDATE table_name
SET column1 = expr1, column2 = expr2, ...
WHERE condition;

UPDATE jugadores
set nombreEquipo = 'Lakers'
WHERE nombre = 'Lebron James';
```


## Borrado de registros. 

* Se basan en la sentencia ```DELETE```

* Elimina todos los registros de la tabla que cumplen la condición indicada.

* Sólo se puede especificar una tabla.

* Si no se especifica condición en el where borraría todos los registros de la tabla, aunque en algunas versiones de DBMS suele mostrar una advertencia en este caso.

```sql
DELETE 
FROM table_name
WHERE column1 = value1;

DELETE FROM jugadores
WHERE nombre = 'Larry Bird';
```


### Subconsultas en UPDATE y DELETE

La cláusula ```WHERE``` funciona de forma análoga a como hemos visto en la realización de consultas. Por tanto es posible establecer filtrados mediante subconsultas; operadores IN, EXISTS, ALL, ANY, etc. 

```sql
DELETE FROM empleados
WHERE CodigoEmpleado NOT IN 
	(SELECT CodigoEmpleadoRepVentas 
		FROM Clientes)
AND puesto = 'Representante Ventas';
```

* La única limitación puede darse si se pretende efectuar modificaciones o eliminaciones sobre una tabla involucrada en la subconsulta.

## Borrados y modificaciones e [integridad referencial](https://www.aulaclic.es/sql/b_8_1_1.htm). Cambios en cascada.

* Cuando las actualizaciones o eliminaciones provocan dejar de cumplir las reglas de integridad de la base de datos (la de entidades, referencial o definida por el usuario), producen error y son rechazadas.

* La eliminación o actualización de registros de una tabla puede provocar también la actualización o eliminación de registros de otras tablas relacionadas según las reglas de integridad referencial que estén definidas (CASCADE, SET NULL, etc.)

* _En Oracle, solo existe la claúsula ON DELETE. Para modelar el comportamiento de ON UPDATE, hay que generarlo por programación._ Además, hay que tener en cuenta que no todos los motores de almacenamiento soportan las referencias, [más info](https://www.arsys.es/blog/programacion/myisam-o-innodb-elige-tu-motor-de-almacenamiento-mysql/)

* Hay que recordar la claúsula ```REFERENCES``` que se utiliza para crear tablas y definir las relaciones de clave ajena - primaria.

```sql
REFERENCES nombre_tabla[(nombre_columna,...)]
		[ON DELETE opcion]
		[ON UPDATE opcion]
```
Las opciones son: CASCADE, SET NULL o NO ACTION para los casos de eliminación o modificación cuando hay registros relacionados. 

### NO ACTION

```sql
CREATE TABLE equipos (
	id NUMBER(4),
	nombre VARCHAR(20),
	ciudad VARCHAR(20),
	categoria NUMBER(2)
) engine = innodb;

CREATE TABLE jugadores (
	id NUMBER(8),
	nombre VARCHAR(20),
	dorsal NUMBER(2),
	posicion VARCHAR(20),
	FOREIGN KEY equipo_id NUMBER(4) REFERENCES equipos(id)
		ON DELETE NO ACTION 
		ON UPDATE NO ACTION
) engine = innodb; # MyISAM NO soporta referencias. 

INSERT INTO equipos VALUES(123, 'Tarazona CF', 'Tarazona', '4');

INSERT INTO jugadores VALUES(456, 'Juan', 7, 'delantero', 123);
INSERT INTO jugadores VALUES(457, 'José', 9, 'delantero', 123);
```

* Si se intenta borrar el equipo 'Tarazona CF', se producirá un error puesto que hay jugadores relacionados con dicho equipo.

```sql
DELETE FROM equipos
WHERE id = 123; # ERROR

DROP table equipos; # ERROR
```
* Si se intenta modificar el id del equipo 'Tarazona CF', se producirá un error puesto que hay jugadores relacionados con dicho equipo.

```sql
UPDATE equipos
SET id = 245 WHERE id = 123; # ERROR
```

### CASCADE

```sql
CREATE TABLE equipos (
	id NUMBER(4),
	nombre VARCHAR(20),
	ciudad VARCHAR(20),
	categoria NUMBER(2)
) engine = innodb;

CREATE TABLE jugadores (
	id NUMBER(8),
	nombre VARCHAR(20),
	dorsal NUMBER(2),
	posicion VARCHAR(20),
	FOREIGN KEY equipo_id NUMBER(4) REFERENCES equipos(id)
		ON DELETE CASCADE 
		ON UPDATE CASCADE
) engine = innodb;

INSERT INTO equipos VALUES(123, 'Tarazona CF', 'Tarazona', '4');

INSERT INTO jugadores VALUES(456, 'Juan', 7, 'delantero', 123);
INSERT INTO jugadores VALUES(457, 'José', 9, 'delantero', 123);
```

* Se borra el equipo 'Tarazona CF', y en consecuencia se borran también los jugadores del equipo.

```sql
DELETE FROM equipos
WHERE id = 123;

## También funciona si se hace DROP
DROP table equipos;
```
* Se modifica el id del equipo 'Tarazona CF', se producirá la actualización en cascada de los jugadores que tuvieran dicho equipo_id, que pasará a ser el nuevo tras la modificación

```sql
UPDATE equipos
SET id = 245 WHERE id = 123;
```

## Subconsultas y composiciones en órdenes de edición.
## Transacciones. Sentencias de procesamiento de transacciones.
## Problemas asociados al acceso simultáneo a los datos.
## Bloqueos compartidos y exclusivos. Políticas de bloqueo.
## Procesos y consultas masivas.