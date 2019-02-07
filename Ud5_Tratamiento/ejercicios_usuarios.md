## Ejercicios sobre la gestión de usuarios en MySQL

1. Crea un usuario 'creausuarios' y otórgale permisos para que pueda crear usuarios.
2. Conecta como 'creausuarios' crea un nuevo usuario 'usujardineria'.
3. Conecta como 'root', otórgale permisos al usuario 'usujardineria' para que pueda crear tablas en jardineria.
4. Conecta como 'usujardineria' comprueba que dispone de los permisos creando una tabla de árboles en jardineria. La tabla debe tener código, nombre, altura máxima y edad de vida media.
5. Conecta como 'root', muestra los permisos que tiene el usuario 'usujardineria'.
6. Conecta como 'usujardineria' muestra los permisos que posee y comprueba que son los mismos a los de la orden anterior.
7. Conecta como 'root' otorga permiso de creación y borrado, así como de ejecución al usuario 'usujardineria' sobre una base de datos creada previamente.
8. Conecta como 'root' crea un usuario de nombre 'creartablasnba' que tenga permisos para crear, borrar y modificar tablas de nba.
9. Conecta como 'creartablasnba' crea un tabla sencilla de al menos dos columnas.
10. Conecta como 'root' crea un usuario de nombre 'soloconsulta' que pueda realizar operaciones de consulta sobre todas las tablas de todas las bases de datos.
11. Añade a 'soloconsulta' el permiso de insertar en la tabla jugadores de nba.
12. Conecta como 'soloconsulta' añade una fila a la tabla arboles creada anteriormente. Intenta borrar la fila creada. Describe la respuesta obtenida.
13. Conecta como 'root' crea un usuario de nombre 'localaccess' que pueda seleccionar todas las tablas de la base de datos jardineria.
14. Conecta como 'root' crea un usuario de nombre 'limitedaccess' que pueda realizar operaciones de inserción, actualización y consulta sobre la primera columna de la tabla creada previamente (arboles).
15. Conéctate como 'limitedaccess' y comprueba que tienes los permisos ejecutando las órdenes SQL SELECT, UPDATE e INSERT.
16. Comprueba qué permisos tienes con el usuario actual.
