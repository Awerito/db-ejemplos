## **Actividad: Normalización con Violaciones de Formas Normales**  

⏳ **Duración estimada: 60 minutos**  
🎯 **Objetivo:** Identificar violaciones en las relaciones proporcionadas y
normalizar las tablas hasta alcanzar la **tercera forma normal (3FN)**.

---

### **Instrucciones:**  
1. **Identifica las violaciones** en las relaciones proporcionadas, que
   incluyen problemas de 1FN, 2FN o 3FN.
2. **Aplica el proceso de normalización** hasta que la relación cumpla con las
   reglas de **1FN**, **2FN**, y finalmente **3FN**.

---

### **Ejercicios con Violaciones de Formas Normales**

📌 **1. Relación de Estudiantes y Cursos**
Tabla sin normalizar:  

| ID Estudiante | Nombre   | Cursos Inscritos             | Fecha Inscripción    |
|---------------|----------|--------------------------    |----------------------|
| 1             | Luis     | Matemáticas, Física          | 2025-03-01           |
| 2             | Ana      | Historia                     | 2025-03-02           |
| 3             | Juan     | Matemáticas, Historia        | 2025-03-03           |
| 4             | Pedro    | Matemáticas, Física, Química | 2025-03-04           |

---

📌 **2. Relación de Pedidos**
Tabla sin normalizar:  

| ID Pedido | Cliente | Productos                        | Precio Total | Fecha Pedido |
|-----------|---------|----------------------------------|--------------|--------------|
| 1         | Juan    | Camisa, Pantalón                 | 40           | 2025-04-01   |
| 2         | María   | Zapatos                          | 50           | 2025-04-02   |
| 3         | Juan    | Camisa, Zapatos, Pantalón        | 90           | 2025-04-02   |


---

📌 **3. Relación de Empleados y Proyectos**
Tabla sin normalizar:  

| ID Empleado | Nombre   | Proyecto     | Horas Trabajadas | Supervisor   | Fecha Asignación |
|-------------|----------|--------------|------------------|--------------|------------------|
| 1           | Pedro    | Proyecto A   | 40               | Carlos       | 2025-03-01       |
| 2           | Ana      | Proyecto B   | 35               | Laura        | 2025-03-02       |
| 3           | Luis     | Proyecto A   | 30               | Carlos       | 2025-03-01       |
| 4           | Pedro    | Proyecto C   | 50               | Pablo        | 2025-03-03       |


---

📌 **4. Relación de Facturas de Compras**
Tabla sin normalizar:  

| ID Factura | Cliente | Producto Comprado   | Cantidad | Precio Unitario | Descuento | Total Factura |
|------------|---------|---------------------|----------|-----------------|-----------|---------------|
| 1          | Juan    | Camisa, Pantalón    | 2, 1     | 20, 30          | 5, 0      | 70            |
| 2          | María   | Zapatos, Sombrero   | 1, 3     | 50, 10          | 0, 2      | 32            |
| 3          | Juan    | Pantalón, Camisa    | 1, 2     | 30, 20          | 0, 5      | 55            |


---

📌 **5. Relación de Cursos y Estudiantes**
Tabla sin normalizar:  

| ID Estudiante | Nombre   | Cursos Inscritos               | Profesor         | Calificación |
|---------------|----------|--------------------------------|------------------|--------------|
| 1             | Luis     | Matemáticas, Historia          | Prof. A, Prof. B | 8, 9         |
| 2             | Ana      | Física, Historia               | Prof. C, Prof. B | 7, 9         |
| 3             | Juan     | Matemáticas, Física            | Prof. A, Prof. C | 6, 7         |

---

📌 **6. Relación de Compras y Proveedores (Violaciones en 1FN y 2FN)**  
Tabla sin normalizar:  

| ID Compra | Proveedor  | Productos                 | Fecha Compra | Total Compra |
|-----------|-----------|---------------------------|--------------|--------------|
| 1         | Proveedor A | Producto X, Producto Y    | 2025-04-01   | 100          |
| 2         | Proveedor B | Producto Z, Producto X    | 2025-04-02   | 80           |
| 3         | Proveedor A | Producto Y                | 2025-04-03   | 50           |

---

📌 **7. Relación de Clientes y Servicios (Violaciones en 2FN y 3FN)**  
Tabla sin normalizar:  

| ID Cliente | Nombre   | Servicios Contratados      | Monto   | Fecha Contratación | Tipo Servicio   |
|------------|----------|----------------------------|---------|--------------------|-----------------|
| 1          | Luis     | Servicio A, Servicio B     | 200     | 2025-03-01         | Premium         |
| 2          | Ana      | Servicio C                 | 100     | 2025-03-02         | Básico          |
| 3          | Juan     | Servicio A, Servicio C     | 180     | 2025-03-03         | Premium         |

---

📌 **8. Relación de Alumnos y Actividades (Violaciones en 1FN y 3FN)**  
Tabla sin normalizar:  

| ID Alumno | Nombre   | Actividades Participadas     | Fecha Participación |
|-----------|----------|------------------------------|---------------------|
| 1         | Luis     | Fútbol, Baloncesto           | 2025-03-01          |
| 2         | Ana      | Voleibol, Fútbol             | 2025-03-02          |
| 3         | Juan     | Baloncesto, Voleibol         | 2025-03-03          |

---

💡 **Consejo:** Recuerda seguir este proceso en tres pasos clave:
1. **Primera Forma Normal (1FN):** Eliminar los atributos multivaluados,
   asegurarse de que todos los valores sean atómicos.
2. **Segunda Forma Normal (2FN):** Asegurarse de que todas las dependencias
   funcionales son completas, es decir, no hay dependencias parciales con
respecto a la clave primaria.
3. **Tercera Forma Normal (3FN):** Eliminar las dependencias transitivas,
   asegurándose de que no haya dependencias de atributos no clave sobre otros
atributos no clave.
