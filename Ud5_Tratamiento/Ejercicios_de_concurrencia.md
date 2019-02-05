## Ejercicios de concurrencia


* Poner la sesión en read uncommited para que se puedan producir los 3 problemas de lectura

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```


### Lectura sucia


usuario A | usuario B
	-- | --
BEGIN | BEGIN Update empleados set salario = salario * 1.1; INSERT into empleados values(23, 'Lopez', 1100) |
 SELECT * from empleados; |
| ROLLBACK;


* El usuario A hace uso de unos datos que no existen puesto que se ha hecho ROLLBACK.


### Lectura no repetible


usuario A | usuario B
		-- | --
	START TRANSACTION;
	Select * from empleados; | |
	| Update empleados set salario = salario * 1.1; |
	Select * from empleados | 

	* Los datos de los registros cambian entre la primera consulta  y la segunda del usuario A.

### Lectura fantasma

usuario A | usuario B
		-- | --
START TRANSACTION;
SELECT * from empleados; | |
| INSERT into empleados values(23, 'Lopez', 1100) |
Select * from empleados | |

	* Aparecen nuevos datos entre las lecturas.