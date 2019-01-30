## Ejercicios

* Hacer [AUTO INCREMENT](http://www.mysqltutorial.org/mysql-reset-auto-increment) los campos código (valor inicial el inmediato superior al mayor que ya posean).

```sql
	#Ejemplo: 
	ALTER TABLE table_name MODIFY COLUMN column_name COLUMN_TYPE auto_increment [PRIMARY KEY] ;
```	

* Insertar nuevos jugadores, insertando únicamente Nombre, procedencia, altura, peso, posicion y equipo. Comprobar si la inserción se hace correctamente y el código de jugador se auto incrementa.


* Configurar modificadores ON UPDATE y ON DELETE (CASCADE) para las 4 tablas.

	- Modificar el nombre de un equipo, qué efectos en las tablas restantes. Detállalos.
	- Eliminar un equipo. Detalla los efectos en las tablas restantes.


* Configurar modificadores ON UPDATE y ON DELETE (SET NULL) para las 4 tablas.

	- Modificar el nombre de un equipo, qué efectos en las tablas restantes. Detállalos.
	- Eliminar un equipo. Detalla los efectos en las tablas restantes.


* Configurar modificadores ON UPDATE y ON DELETE (NO ACTION) para las 4 tablas.

	- Modificar el nombre de un equipo, qué efectos en las tablas restantes. Detállalos.
	- Eliminar un equipo. Detalla los efectos en las tablas restantes.


