# BD Sakila

## Ejercicios

* En primer lugar hay que importar tanto el schema de la BD como los datos que vienen en dos script SQL diferentes.

## Modificaciones

1. La base de datos está un poco anticuada en las fechas. Para actualizarla se propone:
	* Añadir a las columnas de la fecha de alquiler y devolución 13 años sobre sus valores actuales.
	* Añadir 8 meses más a las columnas previamente modificadas.

2. Localizar las ciudades españolas y comprobar si están correctamente escritas. Corregirlas en caso de haber errores.


## Consultas I

1. ¿Qué actores se llaman 'John'?
2. ¿Qué actores se apellidan 'Williams'?
3. ¿Cuántos apellidos distintos de actores hay?
4. ¿Qué apellidos que no se repiten?
5. ¿Qué apellidos aparecen más de una vez?
6. ¿Qué actor aparece en más películas?
7. ¿Está 'Academy Dinosaur' disponible para alquilar desde la Store 1?
8. Inserta un registro para señalar que Mary Smith alquila 'Academy Dinosaur' siendo atendida por Mike Hillyer en la Store 1 hoy.
9. ¿Cuál es la duración media de las películas?
10. ¿Cuál es la duración media de las películas por categoría?
11. ¿Cuántos actores no han actuado en ninguna película?
12. ¿Cuántas películas están alquiladas actualmente?
13. ¿Qué categoría de películas ha generado mayores ingresos?
14. ¿A qué hora se dan los pagos por alquiler con más frecuencia?
15. ¿Por qué la siguiente consulta devuelve 0 filas?

```sql
	select * from film natural join inventory;
```


