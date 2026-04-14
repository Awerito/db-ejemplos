## **Actividad en Clase: Creación del Modelo Académico en SQL (DDL)**

⏳ **Duración total: 90 minutos**  
🎯 **Objetivo:** Traducir un modelo relacional a un script SQL con sentencias `CREATE TABLE`, definiendo claves primarias, foráneas, tipos de datos y restricciones apropiadas.

---

1. **Trabajo individual**  
2. **Ejercicio práctico:** Implementar el DDL de un sistema académico simple compuesto por las tablas `profesor`, `sala`, `curso`, `alumno`, `alumno_curso` y `horario`.
3. El script resultante será el punto de partida de la clase [08-sql-crud](../08-sql-crud/).

### **📋 Instrucciones**

---

### **🏗️ Requisitos del modelo**

✅ Aplica las reglas vistas en clase para definir correctamente el esquema en PostgreSQL.

- Cada tabla debe tener una **clave primaria** adecuada.
- Las relaciones entre tablas deben expresarse con **claves foráneas** (`REFERENCES`).
- Usar tipos de datos apropiados (`INTEGER`, `VARCHAR(n)`, `TIME`, `DATE`, etc.).
- Aplicar `NOT NULL` donde corresponda.
- En `alumno_curso` la PK debe ser compuesta (`alumno`, `curso`).
- En `horario` considerar PK compuesta (`curso`, `dia`, `hora_inicio`) o equivalente que evite solapes obvios.
- Pensar el **orden** de creación: una tabla con FK no puede existir antes que la tabla a la que referencia.

---

### **📝 Entregable**

✅ Un único archivo `modelo.sql` con todas las sentencias `CREATE TABLE` en el orden correcto, ejecutable de principio a fin sobre una base de datos vacía en PostgreSQL.

---

### **👓 Ejercicio Final: Verificación**

✅ Ejecutar el script en una BD limpia y comprobar que:

- No aparecen errores de sintaxis ni de referencia.
- Las 6 tablas existen con sus columnas y restricciones.
- Intentar insertar una fila inválida (p. ej. un `alumno_curso` con `alumno` inexistente) falla por la FK.

---

💬 **Reflexión final**  

- ¿Por qué el orden de creación importa y qué ocurre si se invierte?
- ¿Qué tipo de datos elegiste para `hora_inicio` / `hora_fin` y por qué?
- ¿Qué `ON DELETE` usarías para cada FK (`CASCADE`, `RESTRICT`, `SET NULL`) y cómo justifica el caso de negocio tu elección?
- ¿Qué restricciones adicionales (`CHECK`, `UNIQUE`) podrían prevenir datos inconsistentes?
