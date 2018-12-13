# Realización de consultas
## El lenguaje DML (Data Manipulation Language)
* Agrupa las sentencias SELECT, INSERT, DELETE y UPDATE.
* Están enfocadas a la consulta, inserción, eliminación y modificación de registros presentes en Bases de datos. 
* A cualquier ejecución de un comando en el DBMS se le denomina consulta, no solo a la ejecución de un SELECT.

## La sentencia SELECT
Se utiliza para consultar la información presente en las tablas. Una consulta puede seleccionar información de una sola tabla, de varias, o mediante relaciones entre tablas e incluso mediante tablas virtuales creadas a partir de otra consulta.

## Consultas básicas

Hay elementos que forman parte de todas las consultas, como la instrucción SELECT o la palabra FROM, para indicar el origen de los datos que se van a consultar.

```sql
SELECT [DISTINCT] sql_expr [, sql_expr]... 
[FROM table_name]
[WHERE filter]
```

ejemplos de sql_expr: nombre_columna [AS alias], *, expresión.

Se pueden seleccionar de una tabla una columna, una serie de ellas o todas *. Además, es posible utilizar una expresión algebraica compuesta por operadores, operandos y funciones. DISTINCT es opcional y sirve para eliminar las repeticiones en el resultado de la consulta.




## Filtros

Son Condiciones que cualquier DBMS intepreta para seleccionar registros. La palabra clave asociada a los filtros es WHERE.

```sql
SELECT [DISTINCT] sql_expr [, sql_expr]... 
[FROM table_name]
[WHERE filter]
``` 

El filtro es una expresión que indica la condición (o condiciones) que deben cumplir los registros. Por ejemplo, para consultar los empleados cuyo nombre de pila es Michael:

```sql
SELECT * 
from employees
WHERE firstName = 'Michael'
```

### Expresiones para filtros

*Operandos: Puede ser un número, una cadena de caracteres, el campo firstName u otra expresión. 
	* Los operandos numéricos van sin comillas simples, mientras que los caracteres o fechas van entre comillas simples.
* Operadores aritméticos: +, -, *, /, %. Suma, resta (o signo), producto, división y módulo respectivamente.
* Operadores relacionales: <, >, <>, <=, >=, = 
* Operadores lógicos: AND, OR, NOT.
* Funciones: concat, date_add, right, left... Varían en función del DBMS con el que trabajemos. 
* Uso de paréntesis. Cuando se desea alterar la prioridad natural de los operadores es válido recurrir a los paréntesis para aplicar los filtros.
### Construcción de Filtros
### Con operador de pertenencia a conjuntos
Además de los vistos anteriormente, tenemos el operador IN, de pertenencia a conjuntos. Por ejemplo, para localizar empleados cuyo nombre de pila sea Michael, Peter o John.
```sql
SELECT * 
from employees
WHERE firstName IN ('Michael', 'Peter', 'John')
```

### Test de valor nulo

Mediante estos operadores; *IS* o *IS NOT* se comprueba si un campo es o no nulo.

```sql
SELECT * 
from employees
WHERE driver_license IS null
```


### Rango

Este operador nos devuelve los registros incluidos en el rango expresado mediante el operador *BETWEEN*

```sql
SELECT * 
from employees
WHERE employee_age BETWEEN 45 AND 50
```

### Patrón

Este tipo de filtro selecciona los registros que cumplan las características expresadas por el *patrón*. Permiten el uso de comodines *%* (Cero o más caracteres) y *_* (exactamente un carácter) para localizar cadenas de caracteres. 

```sql
SELECT * 
from employees
WHERE firstName LIKE 'J%'; #Cualquier nombre que empiece por J.

SELECT * 
from employees
WHERE firstName LIKE 'J___n'; #Cualquier nombre que empiece por J, tenga cinco letras y acabe en n.
```

### Límite de registros

Aunque varía en función del DBMS busca limitar el número de registros devueltos por la consulta. Su sintaxix es *[LIMIT [Desplazamiento,] nº de filas]*

```sql
SELECT * 
from employees
WHERE employee_age > 30
LIMIT 4,2 #Devuelve 2 filas a partir de la quinta. Se puede usar sin número de filas.
```

## Ordenar el resultado de las consultas

Mediante la cláusula *ORDER BY* se puede establecer el orden del resultado de la consulta. Admite varias columnas para la ordenación, así como el sentido *ASC* o *DESC* de la misma. Es posible establecer el tipo de ordenación para cada columna.

```sql
SELECT * 
from employees
ORDER BY employee_age, lastName DESC #Si no se indica tipo de orden es ASC por defecto

SELECT * 
from employees
ORDER BY employee_age ASC, lastName DESC
```
