## **Actividad en Clase: Gestión API REST con FastAPI y SQLAlchemy**

⏳ **Duración total: 120 minutos**

🎯 **Objetivo:** Aplicar conocimientos de desarrollo web para construir una API
RESTful en FastAPI que integre el modelo de datos en SQLAlchemy con migraciones
Alembic y funciones CRUD transaccionales.

---

1. **Trabajo individual o en parejas**
2. **Ejercicio práctico:** Crear modelos desde cero con SQLAlchemy, aplicar
   migraciones Alembic y exponer operaciones CRUD mediante FastAPI.
3. Implementar funciones transaccionales con manejo de errores y control de
   `commit/rollback`.

---

### **📋 Instrucciones Generales**

---

#### **1. Modela y expón:**

La empresa ha solicitado una API para gestionar los recursos de un sistema que
tú debes modelar según una plantilla base. A partir del repositorio de ejemplo:

🔗 **Modelo base:** [Diagrama en Pony
Editor](https://editor.ponyorm.com/user/Awerito/GestionBiblioteca/designer)

🔗 **Código base:** [Awerito/ejemplo-sqlalchemy-alembric – branch
`fastapi-ejemplo`](https://github.com/Awerito/ejemplo-sqlalchemy-alembric/tree/fastapi-ejemplo)

Debes construir un sistema siguiendo estos pasos:

* Diseñar los modelos en SQLAlchemy con relaciones y restricciones apropiadas.
* Crear las migraciones usando Alembic de manera pertinente.
* Implementar funciones de inserción, actualización, eliminación y consulta con
manejo de transacciones (`commit/rollback` según corresponda).
* Exponer cada operación como un endpoint usando FastAPI.

---

#### **2. Requisitos mínimos del modelo:**

Tu sistema debe incluir al menos 3 entidades relacionadas y permitir
operaciones completas sobre ellas. Por ejemplo:

* Producto, Categoría y Proveedor.
* Libro, Autor y Usuario.
* Cliente, Pedido y DetallePedido.

---

#### **3. Endpoints esperados:**

* Crear, listar, obtener por ID, actualizar y eliminar para cada entidad
principal.
* Endpoint para operaciones más complejas (consultas filtradas, relaciones,
etc).

---

### **🧪 Simulación de partida**

1. Crea y aplica migraciones iniciales con Alembic.
2. Implementa las funciones de negocio con SQLAlchemy y asegúrate de que usen
   transacciones.
3. Agrega endpoints en FastAPI que conecten con las funciones CRUD.
4. Usa la interfaz de Swagger UI para probar los endpoints (http://localhost:8000/docs).
5. Simula errores y asegúrate de que la API responde correctamente ante fallas
   (rollback, errores HTTP).
