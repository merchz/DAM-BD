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

-Operandos: Puede ser un número, una cadena de caracteres, el campo firstName u otra expresión. 
Los operandos numéricos van sin comillas simples, mientras que los caracteres o fechas van entre comillas simples.

-Operadores aritméticos: +, -, *, /, %. Suma, resta (o signo), producto, división y módulo respectivamente.

-Operadores relacionales: <, >, <>, <=, >=, = 

-Operadores lógicos: AND, OR, NOT.

-Funciones: concat, date_add, right, left... Varían en función del DBMS con el que trabajemos. 

-Uso de paréntesis. Cuando se desea alterar la prioridad natural de los operadores es válido recurrir a los paréntesis para aplicar los filtros.

### Construcción de Filtros


### Filtros con operador de pertenencia a conjuntos
Además de los vistos anteriormente, tenemos el operador IN, de pertenencia a conjuntos. Por ejemplo, para localizar empleados cuyo nombre de pila sea Michael, Peter o John.

```sql
SELECT * 
from employees
WHERE firstName IN ('Michael', 'Peter', 'John')
```

