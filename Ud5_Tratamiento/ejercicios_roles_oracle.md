### Ejercicios roles y perfiles. Describir el resultado obtenido en cada ejercicio.

1. Establece un rol de usuario llamado __administrativo__ que tenga los privilegios sobre *pedidos* y *clientes*. Asimismo crea otro rol llamado __contable__ que tenga todos los privilegios sobre *pagos*. El rol __contable__ deberá identificarse mediante contraseña.
2. Crea un usuario llamado *Juan* que tenga:
	* roles *connect* y *resource*
	* default tablespace *Users*
	* temporary tablespace *temp*
	* 50 MB de *quota* en *Users* 
3. Asigna el rol __administrativo__ a *Juan*.
4. Utilizando el usuario *Juan* y consulta:
	* Nombre de los *clientes* de Madrid 
	* Importe del último pago realizado.
5. Activa el rol __contable__ y repite la consulta del importe anterior.
6. Desactiva los roles de *Juan* incluido el rol por defecto.
7. Vuelve a consultar el nombre de los clientes de Madrid.