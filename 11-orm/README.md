## **Actividad en Clase: Gestión de Biblioteca**

⏳ **Duración total: 120 minutos**

🎯 **Objetivo:** Aplicar conocimientos para desarrollar un modelo por ORM de
gestión de una biblioteca modelando: Libro, Autor, Categoria, Usuario,
Prestamo y Multa.

---

1. **Trabajo individual o parejas**
2. **Ejercicio práctico:** Modelar desde cero con el ORM SQLAlchemy y modelar
   con un esquema mínimo de base de datos para insersiónes y consultas.
3. Crear funciones de validación de la lógica de negocios.

---

### **📋 Instrucciones Generales**

---

#### **1. Modela:**

La Universidad ha encargado el desarrollo de un sistema de gestión para su
biblioteca académica. Este sistema debe permitir llevar el control de los
libros disponibles, los autores involucrados, las categorías temáticas, y los
préstamos realizados por los estudiantes. También debe poder registrar multas
cuando los libros se devuelven tarde.

A continuación, se describen las entidades relevantes del sistema:

* Autor: persona que ha escrito uno o más libros. Un libro puede tener uno o
varios autores.
* Categoría: cada libro pertenece a una categoría como "Ciencia Ficción",
"Programación", "Matemáticas", etc.
* Libro: tiene un título, año de publicación y una categoría. Puede tener
múltiples autores.
* Usuario: corresponde a un estudiante registrado en la biblioteca.
* Préstamo: registro de que un libro fue prestado a un usuario. Tiene fechas de
préstamo y devolución, así como un estado (pendiente, devuelto, retrasado).
* Multa: se genera cuando un préstamo se retrasa más allá de un período de
gracia. Se indica si la multa fue pagada o no (un usuario con multa no puede
pedir nuevos préstamos hasta que la pague).

---

#### **2. Consultas:**

* ¿Qué libros han sido más prestados este semestre?
* ¿Qué usuarios tienen préstamos activos o multas pendientes?
* ¿Cuál es la deuda total de un estudiante?
* ¿Cuántos libros de la categoría “Bases de Datos” fueron prestados este año?

---

### **🧪 Simulación de partida**

1. Realiza las migraciones necesarias hasta dar con un modelo funcional.
2. Inserta datos por medio de tus funciones de inserción.
3. Simula préstamos y devoluciones de libros.
4. Registra multas y verifica que se actualicen correctamente.

---

### **📚 Recursos Adicionales**

- [SQLAlchemy Documentation](https://docs.sqlalchemy.org/en/14/)
- [SQLAlchemy Tutorial](https://www.geeksforgeeks.org/sqlalchemy-tutorial-in-python/)
- [SQLAlchemy Tutorial en Español](https://docs.sqlalchemy.org/en/14/orm/basic_relationships.html)
- [SQLAlchemy Modelamiento en Español](https://j2logo.com/python/sqlalchemy-tutorial-de-python-sqlalchemy-guia-de-inicio/)
