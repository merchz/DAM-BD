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

ejemplos de sql_expr: nombre_columna [AS alias], \* expresión.

Se pueden seleccionar de una tabla una columna, una serie de ellas o todas \*. Además, es posible utilizar una expresión algebraica compuesta por operadores, operandos y funciones. DISTINCT es opcional y sirve para eliminar las repeticiones en el resultado de la consulta.




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

* Operandos: Puede ser un número, una cadena de caracteres, el campo firstName u otra expresión. 
	* Los operandos numéricos van sin comillas simples, mientras que los caracteres o fechas van entre comillas simples.
* Operadores aritméticos: +, -, \*, /, %. Suma, resta (o signo), producto, división y módulo respectivamente.
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

## Funciones de resumen
* Count(expresión)
	* Cuenta el número de valores (excluye nulos) de la columna (o fila) indicada.
* Sum(expresión)
	* Suma los valores pasados como argumento
* Avg(expresión)
	* Calcula el valor medio de los valores del argumento
* Min(expresión)
	* Calcula el mínimo
* Max(expresión)
	* Calcula el máximo

### Agrupación de registros
Además de las consultas que emplean las funciones de resumen, existe la posibilidad de llevar a cabo agrupaciones de registros, es decir, obtener conjuntos de registros que comparten el mismo valor en una o varias columnas. Mediante la cláusula *GROUP BY* es posible definir los agrupamientos que se desean obtener. Por ejemplo:

```sql
SELECT dept_name, count(*) 
from employees
GROUP BY dept_name; # Agrupa por los distintos departamentos
```

### Filtros para Agrupaciones
Del mismo modo que se ha visto que es posible establecer filtros mediante el uso de la cláusula *WHERE* es posible establecer filtros tras llevar a cabo agrupaciones (*WHERE* filtra _antes_ de hacer las agrupaciones). Mediante la cláusula *HAVING* se puede filtrar resultados de consultas que incluyen agrupaciones.

```sql
SELECT dept_name, avg(employee_age)
from employees
GROUP BY dept_name
HAVING avg(employee_age) > 45
ORDER BY avg(employee_age);
```

## Subconsultas
Mediante la anidación de subconsultas es posible filtrar mediante los datos que proporciona otra consulta diferente. Este tipo de filtros pueden aplicarse tanto al filtro de columnas *WHERE* como al de agrupaciones *HAVING*. 

### Pertenencia a conjuntos
```sql
SELECT firstName, lastName
from employees
where dept_name IN (SELECT name from dept where dept_leader_firstName = 'Michael') #empleados que trabajan en los departamentos cuyo jefe se llama Michael
```

### Comparación
Además, es posible establecer comparaciones entre el resultado devuelto por la subconsulta mediante cualquier operador aplicado en los filtros, aunque para estos casos hay que velar porque la subconsulta devuelva una única fila y columna o dará error.

### Operadores de cuantificación *ALL* y *ANY*

Este tipo de cláusulas se emplean para obtener la relación entre una expresión y todos los registros devueltos por una subconsulta (*ALL*) o solo algunos de ellos (*ANY*).

```sql
SELECT firstName, lastName, employee_age
from employees
where employee_age > ALL (SELECT employee_age from employees where dept_name = 'Management')
```

### Existencia
Mediante la cláusula *EXISTS* es posible filtrar los resultados de una consulta siempre y cuando la subconsulta asociada devuelva algún resultado, o lo contrario mediante *NOT EXISTS*.

```sql
select *
from employees E
where EXISTS (select *
                from relationship R 
               where R.employee_id = E.employee_id
                 and R.type = 'brother') #Devolverá los empleados que tengan algún hermano en la empresa
```

## Consultas Multitabla
Se denominan así las que pueden consultar información de más de una tabla. La unión (join) se realiza mediante los campos que tienen en común las tablas. Hay varios tipos de uniones y los iremos viendo poco a poco. 

La unión más habitual es el producto cartesiano y se puede expresar mediante la adición de tablas en la cláusula ```FROM```, por ejemplo:

```sql
SELECT t1.col_a, t2.col_x
from table1 t1, table2 t2
WHERE ...
```

El producto cartesiano de dos o más tablas resulta en la combinación de todas las filas de las tablas involucradas, por lo que suele ser necesario establecer un filtro, en la cláusula ```WHERE``` referenciando las columnas que son comunes en las tablas, a fin de evitar repeticiones. 

Tenemos por lo tanto que en las consultas multitabla se da la presencia de varias tablas en la cláusula ```FROM``` como el filtro que evita redundancias en la cláusula ```WHERE```. A esta combinación se la denomina JOIN y puede abarcar desde 2 a N tablas presentes en la base de datos.

Cuando se emplean más de dos tablas, el número de JOIN también va aumentando. Se entiende que el primer JOIN se produce entre las dos primeras tablas, el segundo reune el resultado del primer JOIN con la tercera tabla y así sucesivamente.

A partir de [SQL2](https://es.wikipedia.org/wiki/SQL#Or%C3%ADgenes_y_evoluci%C3%B3n) (1992) se introdujo una nueva sintaxis para determinados tipos de consultas multitabla. Los JOIN internos, de producto cartesiano (JOIN cruzado) y externos:

1. JOIN interna.
    * Equivalencia (INNER JOIN)
    * Natural (NATURAL JOIN)
2. JOIN cruzado (CROSS JOIN)
3. JOIN externa
    * Por la izquierda (LEFT OUTER JOIN)
    * Por la derecha (RIGHT OUTER JOIN)
    * Total o completa (FULL OUTER JOIN)


[Ejemplos gráficos JOIN](https://ingenieriadesoftware.es/tipos-sql-join-guia-referencia/)

### INNER JOIN

Este tipo de unión se puede expresar de dos maneras, utilizando la coma para separar las tablas en la cláusula ```FROM``` (como se hacía antes de SQL2) o mediante la palabra reservada ```JOIN```. En ambos casos se realiza el producto cartesiano de todos los registros (o filas). Posteriormente se combinan los registros de una tabla con los de la siguiente. Hay que observar que los registros nulos no se combinan.

```sql
#FORMATO SQL2
SELECT column_name(s)
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name;
```

### NATURAL JOIN

Supone una especialización del INNER JOIN ya visto. Si dos tablas están unidas por columnas homónimas, se habla de NATURAL JOIN. Dado que el vínculo en las sentencias NATURAL JOIN se logra utilizando columnas del mismo nombre, los valores en cuestión no se ofrecen por partida doble en el conjunto de resultados, sino que se fusionan en una columna conjunta. 

```sql
SELECT column_name(s)
FROM table1
NATURAL JOIN table2;
```

En este caso, si se selecciona una columna cuyo nombre es compartido en ambas tablas, no es necesario indicar a qué tabla se refiere, ya que el propio operador solo devolverá un campo único por cada pareja de columnas (o campos) que compartan el mismo nombre.

### CROSS JOIN

Devuelve el producto cartesiano de las tablas, valores nulos inclusive.

```sql
#FORMATO SQL2
SELECT column_name(s)
FROM table1
CROSS JOIN table2;
```
### Composiciones externas. 

En este caso no se requiere que haya equivalencias entre las tablas relacionadas. Es decir, el registro se selecciona aunque no haya otro que le corresponda en otra tabla. La diferencia entre una composición u otra radica en si se admiten (o no) y en qué tabla los registros que no tengan correspondencia; si es en la tabla de la izquierda, en la de la derecha o en ambas.

### LEFT OUTER JOIN

### RIGHT OUTER JOIN

### FULL OUTER JOIN

## Recursos
[Importar y exportar Bases de datos por consola en Linux](https://blog.endeos.com/como-importar-y-exportar-bases-de-datos-mysql-por-consola-en-linux/)