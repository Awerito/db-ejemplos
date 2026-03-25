## **Actividad en Clase: Práctica de Consultas, Actualizaciones, Eliminaciones y Vistas en SQL**

⏳ **Duración total: 120 minutos**  
🎯 **Objetivo:** Aplicar conocimientos de SQL para explorar, modificar y
organizar datos en un conjunto de tablas relacionadas, usando consultas
`SELECT`, actualizaciones `UPDATE`, eliminaciones `DELETE` y la creación de una
vista.

---

1. **Trabajo individual**  
2. **Ejercicio práctico:** Utilizar las tablas `alumno`, `alumno_curso`,
   `curso`, `horario`, `profesor` y `sala` para responder una serie de
ejercicios que ponen en práctica las operaciones fundamentales de SQL.
3. Para configurar el ejercicio en tu db usar este script:
   [alumnos.models.inserts.sql](https://gist.github.com/Awerito/1791cd6fb2cba5efa8804bba234895c6#file-alumnos-model-inserts-sql)

### **📋 Instrucciones**

---

### **📝 Consultas (SELECT)**

✅ Utiliza `SELECT` para recuperar datos de una o más tablas mediante combinaciones (`JOIN`) y funciones de agregación.

- Listar todos los alumnos (id y nombre) de la tabla `alumno`.
- Mostrar todos los cursos junto al nombre de su profesor.
- Para un alumno dado (`id = X`), recuperar los nombres de los cursos en los que está inscrito.
- Contar cuántos alumnos hay inscritos en cada curso.
- Obtener el horario completo (día, hora inicio, hora fin y sala) del curso de “Programación”.
- Listar las salas que están libres los martes (es decir, aquellas que no aparecen en ningún `horario` para ‘Martes’).

---

### **🔄 Actualizaciones (UPDATE)**

✅ Aplica sentencias `UPDATE` para modificar datos existentes, con condiciones que aseguren que la actualización es correcta.

- Cambiar el nombre de un alumno cuyo `id = 10` a “Javier Díaz Fernández”.
- Aumentar en un 5 % la capacidad de todas las salas con capacidad menor a 30.
- Reasignar el curso con `id = 6` para que esté a cargo del profesor con `id = 3`.
- Ajustar la hora de inicio de las clases de ‘Filosofía’ sumando 15 minutos.

---

### **❌ Eliminaciones (DELETE)**

✅ Emplea `DELETE` para eliminar datos específicos, asegurando que las condiciones estén bien definidas.

- Borrar la inscripción del alumno con `id = 4` para el curso con `id = 8`.
- Eliminar todos los horarios correspondientes al curso con `id = 2`.
- Suprimir todos los cursos impartidos por el profesor con `id = 5`.

---

### **👓 Ejercicio Final: Creación de Vista Unificada**

✅ Define una **vista** llamada `vista_global_academica` que integre la información de todas las tablas en una consulta consolidada.

<!-- ```sql -->
<!-- CREATE VIEW academic_overview AS -->
<!-- SELECT -->
<!--   a.id             AS alumno_id, -->
<!--   a.nombre         AS alumno_nombre, -->
<!--   c.id             AS curso_id, -->
<!--   c.nombre         AS curso_nombre, -->
<!--   p.nombre         AS profesor_nombre, -->
<!--   h.dia            AS dia, -->
<!--   h.hora_inicio    AS inicio, -->
<!--   h.hora_fin       AS fin, -->
<!--   s.nombre         AS sala_nombre, -->
<!--   s.capacidad      AS sala_capacidad -->
<!-- FROM alumno_curso ac -->
<!-- JOIN alumno a    ON ac.alumno = a.id -->
<!-- JOIN curso c     ON ac.curso = c.id -->
<!-- JOIN profesor p  ON c.profesor = p.id -->
<!-- JOIN horario h   ON c.id = h.curso -->
<!-- JOIN sala s      ON h.sala = s.id; -->
<!-- ``` -->

---

💬 **Reflexión final**  

- ¿Qué dificultades encontraste al unir varias tablas?  
- ¿Qué ventajas tiene una vista como `vista_global_academica` para futuras consultas o reportes?  
- ¿Qué prácticas debes tener para evitar errores al hacer `UPDATE` o `DELETE`?  

---


## **Actividad en Clase: Práctica de Consultas, Actualizaciones, Eliminaciones y Vistas en SQL**

⏳ **Duración total: 120 minutos**  
🎯 **Objetivo:** Aplicar conocimientos de SQL para explorar, modificar y organizar datos en un conjunto de tablas relacionadas, usando consultas `SELECT`, actualizaciones `UPDATE`, eliminaciones `DELETE` y la creación de una vista.

---

1. **Trabajo individual**  
2. **Ejercicio práctico:** Utilizar las tablas `autor`, `categoria`, `libro`, `autor_libro`, `usuario`, `prestamo`, `multa` para responder una serie de ejercicios que ponen en práctica las operaciones fundamentales de SQL.
3. Para configurar el ejercicio en tu db usar este script:
   [SQL Modelo de Biblioteca](https://gist.github.com/Awerito/353adc109f00d3ff6b27006ed92d10f4)

### **📋 Instrucciones**

---

### **📝 Consultas (SELECT)**

✅ Utiliza `SELECT` para recuperar datos de una o más tablas mediante combinaciones (`JOIN`) y funciones de agregación.

- Obtener los usuarios con más préstamos retrasados.

```sql
SELECT usuario.id, usuario.nombre, COUNT(*) AS prestamos_retrasados
FROM prestamo
JOIN usuario ON prestamo.usuarios = usuario.id
WHERE prestamo.estado = 'retrasado'
GROUP BY usuario.id
ORDER BY prestamos_retrasados DESC;
```

- Libros con más de 1 autor.

```sql
SELECT libro.id, libro.titulo, COUNT(*) AS num_autores
FROM autor_libro
JOIN libro ON autor_libro.libro = libro.id
GROUP BY libro.id
HAVING COUNT(*) > 1;
```

- Total de multas por usuario.

```sql
SELECT usuario.id, usuario.nombre, SUM(multa.monto) AS total_multas
FROM multa
JOIN prestamo ON multa.prestamo_usuarios = prestamo.usuarios AND multa.prestamo_libros = prestamo.libros
JOIN usuario ON prestamo.usuarios = usuario.id
GROUP BY usuario.id;
```

- Libros por categoría y año de publicación.

```sql
SELECT categoria.nombre AS categoria, EXTRACT(YEAR FROM libro.publicacion) AS año, COUNT(*) AS num_libros
FROM libro
JOIN categoria ON libro.categoria = categoria.id
GROUP BY categoria.nombre, EXTRACT(YEAR FROM libro.publicacion)
ORDER BY categoria.nombre, año;
```

- Libros prestados en el año 2023.

```sql
SELECT libro.id, libro.titulo, COUNT(*) AS num_prestamos
FROM prestamo
JOIN libro ON prestamo.libros = libro.id
WHERE EXTRACT(YEAR FROM prestamo.prestamo) = 2023
GROUP BY libro.id;
```

<!-- - Promedio de multas pagadas vs no pagadas.

```sql
SELECT
  AVG(CASE WHEN multa.pagado = TRUE THEN multa.monto ELSE 0 END) AS promedio_pagadas,
  AVG(CASE WHEN multa.pagado = FALSE THEN multa.monto ELSE 0 END) AS promedio_no_pagadas
FROM multa;
``` -->

---

### **🔄 Actualizaciones (UPDATE)**

✅ Aplica sentencias `UPDATE` para modificar datos existentes, con condiciones que aseguren que la actualización es correcta.

- Cambiar estado de préstamos a “retrasado” si la fecha de devolución es mayor a 15 días de la fecha préstamo.

```sql
UPDATE prestamo
SET estado = 'retrasado'
WHERE devolucion IS NOT NULL
  AND devolucion > prestamo.prestamo + INTERVAL '15 days'
  AND estado != 'retrasado';
```

- Registrar devolución de un préstamo y cambiar el estado.

```sql
UPDATE prestamo
SET devolucion = CURRENT_DATE, estado = 'devuelto'
WHERE usuarios = [ID_USUARIO]
  AND libros = [ID_LIBRO]
  AND estado = 'pendiente';
```

- Marcar multa como pagada.

```sql
UPDATE multa
SET pagado = TRUE
WHERE id = [ID_MULTA];
```

---

### **❌ Eliminaciones (DELETE)**

✅ Emplea `DELETE` para eliminar datos específicos, asegurando que las condiciones estén bien definidas.

- Eliminar todos los préstamos devueltos hace más de 1 año.

```sql
DELETE FROM prestamo
WHERE devolucion IS NOT NULL
  AND devolucion < CURRENT_DATE - INTERVAL '1 year';
```

- Eliminar libros sin autores asignados (JOIN con `autor_libro`).

```sql
DELETE FROM libro
WHERE id NOT IN (SELECT libro FROM autor_libro);
```

---

### **👓 Ejercicio Final: Creación de Vista Unificada**

✅ Define una **vista** llamada `vista_global_biblioteca` que integre la información de todas las tablas en una consulta consolidada.

```sql
CREATE VIEW vista_global_biblioteca AS
SELECT
  usuario.id AS usuario_id,
  usuario.nombre AS usuario_nombre,
  libro.id AS libro_id,
  libro.titulo AS libro_titulo,
  autor.nombre AS autor_nombre,
  categoria.nombre AS categoria_nombre,
  prestamo.prestamo AS fecha_prestamo,
  prestamo.devolucion AS fecha_devolucion,
  prestamo.estado AS estado_prestamo,
  multa.monto AS monto_multa
FROM
  prestamo
JOIN usuario ON prestamo.usuarios = usuario.id
JOIN libro ON prestamo.libros = libro.id
JOIN categoria ON libro.categoria = categoria.id
JOIN autor_libro ON libro.id = autor_libro.libro
JOIN autor ON autor_libro.autor = autor.id
LEFT JOIN multa ON prestamo.usuarios = multa.prestamo_usuarios AND prestamo.libros = multa.prestamo_libros;
```

---

💬 **Reflexión final**  

- ¿Qué dificultades encontraste al unir varias tablas?  
- ¿Qué ventajas tiene una vista como `vista_global_biblioteca` para futuras consultas o reportes?  
- ¿Qué prácticas debes tener para evitar errores al hacer `UPDATE` o `DELETE`?  
