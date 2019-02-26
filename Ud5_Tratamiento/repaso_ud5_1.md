## Ejercicios repaso

1. Crea una copia de seguridad de la estructura y contenido actual de la BD Jardineria.
2. Modifica la tabla DetallePedido para insertar una columna para el IV de tipo numérico. Mediante una transacción establece el valor de dicho campo a 18 para aquellos registros cuyo pedido tenga fecha a partir del 1 de Julio de 2010. A continuación, actualiza el resto de pedidos estableciendo el IVA a 21.
3. Modifica la tabla DetallePedido para incorporar una columna llamada pvp_linea de tipo numérico, y actualiza todos sus registros para calcular su valor con la fórmula ```pvp_linea = PrecioUnidad * Cantidad*IVA/100```.
4. Borra el cliente que posea el límite de crédito más bajo. ¿Es posible borrarlo con solo una consulta? Justifica tu respuesta.
5. Inserta dos clientes nuevos para un empleado cualquiera que sea representante de ventas. A continuación inserta mediante transacciones un pedido que tenga al menos 3 líneas de detalle. 
6. Modifica el precio de los artículos con precio superior a 200€ rebajándolos un 5%.
7. Crea una vista que devuelva el cliente que más dinero ha gastado en total. LLámala *cliente_top*.


## Índices

Tipos de índices en MySQL

Los índices son un grupo de datos vinculado a una o varias columnas que almacena una relación entre el contenido y la fila en la que se encuentra. Con esto se agilizan las búsquedas en una tabla al evitar que MySQL tenga que recorrer toda la tabla para obtener los datos de la consulta.

Su importancia radica en que sirven para acelerar las consultas a base de datos, sobre todo cuando hay tablas de gran tamaño. Pero los índices no son la solución para todo ya que cambian cada vez que la columna asociada se modifica y no se deberían crear indices sobre columnas en las que son frecuentes las operaciones de escritura. También hay que tener en cuenta que los índices ocupan espacio, a veces incluso más que la tabla que referencian.

En MySQL hay cinco tipos de índices:

* KEY o INDEX: Son usados indistintamente por MySQL, permite crear indices sobre una columna, sobre varias columnas o sobre partes de una columna.
* PRIMARY KEY: Este índice se ha creado para generar consultas especialmente rápidas, debe ser único y no se admite el almacenamiento de NULL.
* UNIQUE: Este tipo de índice no permite el almacenamiento de valores iguales.
* FULLTEXT: Permiten realizar búsquedas de palabras. Sólo pueden usarse sobre columnas CHAR, VARCHAR o TEXT
* SPATIAL: Este tipo de índices solo puede usarse sobre columnas de datos geométricos (spatial) y en el motor MyISAM

[Más info índices](https://www.adictosaltrabajo.com/2015/09/11/introduccion-a-indices-en-mysql/)

1. Localiza los índices ya presentes en la BD employees. ¿Cuáles son y a qué campos afectan?
2. Explica brevemente la utilidad de introducir índices en la BD.
3. Indica cómo mejorarías el rendimiento de consultas de empleados por apellidos.