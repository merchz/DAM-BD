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


## Inserción de registros. 

* Se basan en la sentencia ```INSERT INTO```. Los valores pueden ser cualquier constante, expresión o función o bien las palbaras clave *DEFAULT* o *NULL*.

* Se ha de especificar valor para todos los atributos de la lista de atributos y manteniendo el orden en el que aparecen.

* Si hay algún atributo de la tabla que no aparece en la lista, se le asigna el valor por defecto que tenga y si no tiene se deja a *null*.

* Si no se especifica la lista de atributos, se ha de dar valor a todos los atributos de la tabla en el orden especificado cuando se creó la tabla.

* Si se intenta insertar más valores que columnas tenga la tabla, se producirá un error.


### Un solo registro

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

### Varios registros

* Es posible insertar más de un registro en un solo ```ÌNSERT INTO```.

```sql
INSERT INTO jugadores(nombre, apellido, posicion)
VALUES 	('Pau', 'Gasol', 'Pivot'),
		('Luka','Dončić','Alero'),
		('Stephen','Curry','Base');
```

### Inserción a partir de una consulta.

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

### SET NULL

* Establece a NULL el valor de la clave secundaria cuando se elimina el registro en la tabla principal o se modifica el valor del campo referenciado.

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
		ON DELETE SET NULL
		ON UPDATE SET NULL
) engine = innodb;

INSERT INTO equipos VALUES(123, 'Tarazona CF', 'Tarazona', '4');

INSERT INTO jugadores VALUES(456, 'Juan', 7, 'delantero', 123);
INSERT INTO jugadores VALUES(457, 'José', 9, 'delantero', 123);
```

* Mediante el código bajo estas líneas, la columna equipo_id de la tabla jugadores establecerá los valores a NULL en todas las filas cuyo equipo_id sea igual a 123. Y como es evidente, se borrará en equipos el que tenga el id 123.

```sql
DELETE FROM equipos
WHERE id = 123;
```
* Es conveniente asegurar que las claves ajenas establecidas con este comportamiento no tengan la restricción de NOT NULL o se pueden producir errores.


### RESTRICT

El comportamiento por defecto, es decir, actúa si no se indica otro. Impide realizar modificaciones que violen la integridad referencial de la base de datos.

```sql
DELETE FROM equipos
WHERE id = 123;
```

Vemos que el registro no se puede borrar porque existen registros relacionados en la tabla de jugadores con el equipo con id 123.


## Transacciones. Sentencias de procesamiento de transacciones.

*Una transacción es una interacción con una estructura de datos compleja, compuesta por varios procesos que se han de aplicar uno después del otro. La transacción debe realizarse de una sola vez y sin que la estructura a medio manipular pueda ser alcanzada por el resto del sistema hasta que se hayan finalizado todos sus procesos.* [Fuente](https://es.wikipedia.org/wiki/Transacción_(informática))

El ejemplo clásico de transacción es una transferencia bancaria, en la que se quita saldo a una cuenta y se añade en otra. Si no se puede abonar el dinero en la cuenta de destino, no se debe quitar de la cuenta de origen.

### Principios ACID

Las transacciones *deberían* garantizar todas las propiedades ACID (en ocasiones alguna de estas propiedades se simplifica o debilita en pro de un mejor rendimiento). El significado de las propiedades es:

* Atomicity - Atomicidad. 
Es la propiedad que asegura que la operación se ha realizado o no, y por lo tanto ante un fallo del sistema no puede quedar a medias.

* Consistency - Consistencia. 
Esta propiedad esta ligada a la integridad referencial, es decir solo se pueden escribir datos válidos respetando los tipos de datos declarados y la integridad referencial.

* Isolation - Aislamiento. 
Asegura que una operación no puede afectar a otras. Con esto se asegura que varias transacciones sobre la misma información sean independientes y no generen ningún tipo de error.

* Durability - Durabilidad. 
Cuando se completa una transacción con éxito los cambios se vuelven permanentes.


### Garantía de cumplimiento de las transacciones

Lo más habitual en los DBMS es que tengan activado ```AUTOCOMMIT```, lo que significa que cada comando sql se considera una transacción independiente. Por el contrario, si se desea considerar varias operaciones como una sola hay que establecer ```AUTOCOMMIT = 0``` e indicar manualmente el inicio y final de cada transacción.

El inicio se especifica mediante ```START TRANSACTION``` mientras que el final puede hacerse de dos formas, dependiendo del resultado de las operaciones:

* ```COMMIT``` (se ejecutan todas las instrucciones y guardamos los datos) 

* ```ROLLBACK``` (se produce un error y no se guardan los cambios). 

```sql
 SET AUTOCOMMIT=0; # No recomendado

# START TRANSACTION también desactiva el autocommit y es la forma recomendada de hacerlo
START TRANSACTION;

# distintas operaciones de la transacción...

COMMIT; # Confirma que todo ha ido bien y da por finalizada la transacción.
```
Hay que tener en cuenta que al realizar una transacción SQL que cuando se realice un INSERT, UPDATE o DELETE se generará un bloqueo sobre la tabla y que otros clientes no pueden acceder esta para escribir. Pero si podrán realizar lecturas, en las que no podrán ver los datos del primer cliente hasta que los mismos sean confirmados.

### Soporte a transacciones en MySQL

En MySQL hay que utilizar innodb como motor de almacenamiento para poder manejar transacciones.


## Problemas asociados al acceso simultáneo a los datos.

Asociado al uso de transacciones, aparecen en este contexto los problemas de concurrencia.

*Concurrencia se refiere a la habilidad de distintas partes de un programa, algoritmo, o problema de ser ejecutado en desorden o en orden parcial, sin afectar el resultado final.*[Fuente](https://es.wikipedia.org/wiki/Concurrencia_(inform%C3%A1tica)).

De forma más relacionada con el manejo de transacciones en los DBMS, se producen problemas cuando dos o más transacciones tratan de acceder al mismo dato. En el ámbito de la concurrencia puede darse una serie de problemas:

* Problema de la modificación perdida: 2 transacciones acceden a la misma fila y modifican su valor. La última modificación sobreescribe la anterior.

* Lectura sucia (DIRTY READ): Una transacción modifica una fila, Una segunda transacción lee esa fila antes de que la primera haga COMMIT. Si la primera hace ROLLBACK, la información leída es incorrecta.

* Lectura no repetible (NONREPEATEABLE READ): Una transacción lee una fila. Una segunda transacción modifica esa fila. Las siguientes lecturas de la primera transacción producen resultados diferentes al de la primera lectura.


* Lectura fantasma (PHANTOM READ): Una transacción lee un conjunto de filas. Una segunda transacción modifica los datos. Si la primera transacción repite la lectura con las mismas condiciones de búsqueda, el numero de filas será diferente.



### Niveles de aislamiento

*De las cuatro propiedades ACID de un Sistema de gestión de bases de datos relacionales (DBMS) la de aislamiento es la que más frecuentemente se relaja. Para obtener el mayor nivel de aislamiento, un DBMS generalmente hace un bloqueo de los datos o implementa un Control de concurrencia mediante versiones múltiples (MVCC), lo que puede resultar en una pérdida de concurrencia. Por ello se necesita añadir lógica adicional al programa que accede a los datos para su funcionamiento correcto.*

*La mayor parte de los DBMS ofrecen unos ciertos niveles de aislamiento que controlan el grado de bloqueo durante el acceso a los datos. Para la mayor parte de aplicaciones, el acceso a los datos se puede realizar de modo que se eviten altos niveles de aislamiento (i.e. nivel SERIALIZABLE), reduciendo así la sobrecarga debida a la necesidad de bloqueos por el sistema. El programador debe analizar detenidamente el código que accede a la base de datos para asegurarse de que el descenso del nivel de aislamiento que ofrece el DBMS no produce errores en el programa. Recíprocamente, si se usan altos niveles de aislamiento la posibilidad de bloqueo aumenta, lo que también requiere análisis cuidadoso del código.* [Fuente](https://es.wikipedia.org/wiki/Aislamiento_(ACID)#Niveles_de_aislamiento)


* Read uncommited

	* Permite a las transacciones leer los datos actualizados por otra transacción aún sin terminar las sentencias SELECT son ejecutadas sin realizar bloqueos.
	* Dado que no bloquea nada, no aísla, siendo el más rápido.
	* Pueden ocurrir 3 problemas de concurrencia:
		* Lectura sucia.
		* Lectura no repetible.
		* Lectura fantasma.


* Read commited

	* No permite lecturas sucias, pues bloquea todos los registros actualizados por la transacción. 
	* Los datos leídos pueden ser modificados por otras transacciones
	* Es el que se establece por defecto en Oracle o SQL Server.
	* Pueden ocurrir 2 problemas de concurrencia:
		* Lectura no repetible.
		* Lectura fantasma.


* Repeateable read

	* Es el que se usa por defecto en InnoDB (MySQL).
	* Soluciona el problema de los datos repetibles pero no el de las modificaciones fantasma.
	* Da por definitiva la primera lectura. De este modo, ningún registro leído puede ser cambiado por otra transacción.


* Serializable

	* Penaliza el rendimiento
	* Puede provocar la aparición de interbloqueos (una transacción no pueda finalizar nunca debido a que otra lo está bloqueando indefinidamente).
	* Todas las transacciones se realizan sin concurrencia.
	* Evita todos los problemas de aislamiento.

#### Conocer/Modificar el nivel de aislamiento actual

* Nivel de aislamiento global 

	* ```SELECT @@global.transaction_isolation;```

* Nivel de aislamiento de la sesión actual

	* ```SELECT @@transaction_isolation;```

* Se puede cambiar el nivel de aislamiento global o de una sesión individual mediante:

```sql
SET [SESSION | GLOBAL] TRANSACTION ISOLATION LEVEL 

	READ UNCOMMITTED
	READ COMMITTED
	REPEATABLE READ
	SERIALIZABLE
```

## Gestión de usuarios y privilegios

[MySQL PDF ES](http://www.personal.fi.upm.es/~lmengual/GESTION_BD/GBD_GESTION_USUARIOS.pdf)

[Oracle ES](https://jorgesanchez.net/manuales/abd/control-usuarios-oracle.html)

### Creación de usuarios

```sql
CREATE USER nb_usuario IDENTIFIED BY 'password' [opciones_según_dbms];
```

### Modificación de usuarios

```sql
#MySQL
UPDATE mysql.USER
SET campo = valor
WHERE user='nb_usuario';

#Oracle
ALTER USER nb_usuario
IDENTIFIED BY 'nuevopass' DEFAULT TABLESPACE 'nb_tablespace';
```

### Privilegios en Oracle

* Clasifica los permisos en dos tipos:
	* Sistema.
	* Objeto. 

* Establece *ROLES*. Conjunto de privilegios asignable a un usuario determinado. Un usuario puede tener asignados varios roles.

* Establece *PERFILES*. Un perfil es un conjunto de restricciones en el acceso a recursos. Solo puede haber un perfil por usuario y el objetivo general de definir perfiles es limitar los recursos del sistema disponibles para los usuarios.

De lo anterior se entiende que se pueden asignar a un usuario privilegios de sistema, de objetos, roles y perfil.


## Optimización de consultas

* [MySQL Proceso de ejecución de consultas](http://www.sgmendez.com/2011/03/14/funcionamiento-proceso-consulta-mysql/)
* [Optimización MySQL Adictos al trabajo](https://www.adictosaltrabajo.com/2016/10/24/optimizacion-de-consultas-en-mysql/)

* [Índices](https://jorgesanchez.net/manuales/sql/ddl-otros-sql2016.html)
* [Optimización mediante índices](https://www.siteground.es/kb/optimizacion-mysql-usando-indexes/)
* [Consejos optimización](https://purolinux.com/2017/06/06/20-consejos-para-optimizar-consultas-sql/)
* [Repo Git BD employees MySQL con datos](https://github.com/datacharmer/test_db)
