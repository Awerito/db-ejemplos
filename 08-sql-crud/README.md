## **Actividad en Clase: Práctica de Consultas, Actualizaciones, Eliminaciones y Vistas en SQL**

⏳ **Duración total: 120 minutos**  
🎯 **Objetivo:** Aplicar conocimientos de SQL para explorar, modificar y
organizar datos en un conjunto de tablas relacionadas, usando consultas
`SELECT`, actualizaciones `UPDATE`, eliminaciones `DELETE` y la creación de una
vista.

---

1. **Trabajo individual**  
2. **Pre-requisito:** Tener creado el modelo académico de la clase
   [07-sql-creacion-tablas](../07-sql-creacion-tablas/) (tablas `alumno`,
   `alumno_curso`, `curso`, `horario`, `profesor`, `sala`).
3. **Ejercicio práctico:** Poblar la base con datos de prueba y responder
   los ejercicios que siguen.
4. Para cargar datos de prueba usar este script:
   [alumnos.models.inserts.sql](https://raw.githubusercontent.com/Awerito/base-de-datos-apuntes/refs/heads/master/07-sql-intro/alumnos.model.inserts.sql)

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
- Aumentar en un 5 % la capacidad de todas las salas con capacidad menor a 30.
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
