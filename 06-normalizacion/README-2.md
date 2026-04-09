## **Actividad: Normalización Avanzada y Modelado en Pony Editor**

⏳ **Duración estimada: 2.5 horas**  
🎯 **Objetivo:** Identificar violaciones en las relaciones proporcionadas,
normalizar hasta **3FN**, y modelar el resultado final en
[Pony Editor](https://editor.ponyorm.com/).

---

### **Instrucciones:**
1. **Identifica las dependencias funcionales** de cada relación.
2. **Identifica las violaciones** de 1FN, 2FN y 3FN.
3. **Aplica el proceso de normalización** paso a paso hasta alcanzar **3FN**.
4. **Modela el resultado final** en
   [Pony Editor](https://editor.ponyorm.com/), incluyendo las entidades,
   atributos, claves primarias y relaciones entre tablas.

---

### **Ejercicios**

📌 **1. Citas en una Clínica Dental**

Una clínica dental registra las citas de sus pacientes en una sola tabla.

| No. Dentista | Nombre Dentista | No. Paciente | Nombre Paciente | Fecha Cita  | Hora Cita | No. Consultorio |
|--------------|-----------------|--------------|-----------------|-------------|-----------|-----------------|
| D101         | Tony García     | P100         | Silvia Blanco   | 2025-08-12  | 10:00     | C10             |
| D101         | Tony García     | P105         | Javier López    | 2025-08-13  | 12:00     | C15             |
| D102         | Elena Pérez     | P108         | Iván Muñoz      | 2025-09-12  | 10:00     | C10             |
| D102         | Elena Pérez     | P108         | Iván Muñoz      | 2025-09-14  | 10:00     | C10             |
| D103         | Roberto Solís   | P105         | Javier López    | 2025-10-14  | 16:30     | C15             |
| D103         | Roberto Solís   | P110         | Juan Morales    | 2025-10-15  | 18:00     | C13             |

**Dependencias funcionales:**
- {No. Dentista, Fecha Cita, Hora Cita} → No. Paciente  
- No. Dentista → Nombre Dentista  
- No. Paciente → Nombre Paciente  
- {No. Dentista, Fecha Cita} → No. Consultorio

> **Pista:** La clave primaria es compuesta. Analiza cuáles atributos dependen
> de toda la clave y cuáles solo de una parte.

<!--
RESPUESTA EJERCICIO 1:

Ya está en 1FN (todos los valores son atómicos).

PK: {No. Dentista, Fecha Cita, Hora Cita}

Violaciones de 2FN (dependencias parciales):
- No. Dentista → Nombre Dentista (depende solo de parte de la PK)
- No. Paciente → Nombre Paciente (No. Paciente depende de la PK, pero
  Nombre Paciente depende de No. Paciente, no de la PK)
- {No. Dentista, Fecha Cita} → No. Consultorio (depende solo de parte de la PK)

Descomposición a 2FN:
- Dentistas(No. Dentista PK, Nombre Dentista)
- Pacientes(No. Paciente PK, Nombre Paciente)
- Consultorios_Asignados(No. Dentista PK/FK, Fecha Cita PK, No. Consultorio)
- Citas(No. Dentista PK/FK, Fecha Cita PK, Hora Cita PK, No. Paciente FK)

3FN: Ya no hay dependencias transitivas. El esquema está en 3FN.
-->

---

📌 **2. Agencia de Personal Hotelero**

Una agencia de empleo temporal asigna empleados a hoteles mediante contratos.

| NIF        | No. Contrato | Horas/Semana | Nombre Empleado | No. Hotel | Ciudad Hotel | Categoría Hotel |
|------------|--------------|--------------|-----------------|-----------|--------------|-----------------|
| 11356700A  | C1024        | 16           | Juan Pérez      | H25       | Guadalajara  | 5 estrellas     |
| 23411100B  | C1024        | 24           | Diana Herrera   | H25       | Guadalajara  | 5 estrellas     |
| 71267000C  | C1025        | 28           | Sara Blanco     | H04       | Monterrey    | 4 estrellas     |
| 11356700A  | C1025        | 16           | Juan Pérez      | H04       | Monterrey    | 4 estrellas     |
| 45890100D  | C1026        | 20           | María Torres    | H25       | Guadalajara  | 5 estrellas     |
| 71267000C  | C1026        | 32           | Sara Blanco     | H25       | Guadalajara  | 5 estrellas     |

**Dependencias funcionales:**
- {NIF, No. Contrato} → Horas/Semana  
- NIF → Nombre Empleado  
- No. Contrato → No. Hotel, Ciudad Hotel, Categoría Hotel  
- No. Hotel → Ciudad Hotel, Categoría Hotel  

> **Pista:** Hay dependencias parciales Y transitivas. Identifica ambas.

<!--
RESPUESTA EJERCICIO 2:

Ya está en 1FN.

PK: {NIF, No. Contrato}

Violaciones de 2FN (dependencias parciales):
- NIF → Nombre Empleado
- No. Contrato → No. Hotel, Ciudad Hotel, Categoría Hotel

Descomposición a 2FN:
- Empleados(NIF PK, Nombre Empleado)
- Contratos(No. Contrato PK, No. Hotel FK, Ciudad Hotel, Categoría Hotel)
- Asignaciones(NIF PK/FK, No. Contrato PK/FK, Horas/Semana)

Violaciones de 3FN (dependencias transitivas):
- En Contratos: No. Contrato → No. Hotel → Ciudad Hotel, Categoría Hotel

Descomposición a 3FN:
- Empleados(NIF PK, Nombre Empleado)
- Hoteles(No. Hotel PK, Ciudad Hotel, Categoría Hotel)
- Contratos(No. Contrato PK, No. Hotel FK)
- Asignaciones(NIF PK/FK, No. Contrato PK/FK, Horas/Semana)
-->

---

📌 **3. Catálogo de Películas**

Una base de datos de películas almacena información de títulos, estudios y
actores. Un título puede tener versiones en diferentes años. Un estudio tiene
una sola dirección.

| Título              | Año  | Duración | Tipo  | Estudio | Dirección Estudio | Actor            | Rol           |
|---------------------|------|----------|-------|---------|--------------------|------------------|---------------|
| Star Wars           | 1977 | 124      | Color | Fox     | Hollywood          | Carrie Fisher    | Leia          |
| Star Wars           | 1977 | 124      | Color | Fox     | Hollywood          | Mark Hamill      | Luke          |
| Star Wars           | 1977 | 124      | Color | Fox     | Hollywood          | Harrison Ford    | Han Solo      |
| Mighty Ducks        | 1991 | 104      | Color | Disney  | Buena Vista        | Emilio Estévez   | Gordon        |
| Mighty Ducks        | 1991 | 104      | Color | Disney  | Buena Vista        | Joshua Jackson   | Charlie       |
| Ben Hur             | 1959 | 212      | Color | MGM     | Hollywood          | Charlton Heston  | Judah         |
| Ben Hur             | 1959 | 212      | Color | MGM     | Hollywood          | Martha Scott     | Miriam        |
| El Retorno del Jedi | 1983 | 131      | Color | Fox     | Hollywood          | Carrie Fisher    | Leia          |
| El Retorno del Jedi | 1983 | 131      | Color | Fox     | Hollywood          | Harrison Ford    | Han Solo      |

**Dependencias funcionales:**
- {Título, Año} → Duración, Tipo, Estudio  
- Estudio → Dirección Estudio  
- {Título, Año, Actor} → Rol

> **Pista:** La clave candidata es {Título, Año, Actor}. Hay dependencias
> parciales (atributos que dependen solo de {Título, Año}) y transitivas
> (Dirección Estudio depende de Estudio, no de la clave).

<!--
RESPUESTA EJERCICIO 3:

Ya está en 1FN.

PK: {Título, Año, Actor}

Violaciones de 2FN (dependencias parciales):
- {Título, Año} → Duración, Tipo, Estudio (no dependen de Actor)

Descomposición a 2FN:
- Películas(Título PK, Año PK, Duración, Tipo, Estudio)
- Reparto(Título PK/FK, Año PK/FK, Actor PK, Rol)

Violaciones de 3FN (dependencias transitivas):
- En Películas: Estudio → Dirección Estudio
  (Dirección Estudio depende de Estudio, que no es clave)

Descomposición a 3FN:
- Películas(Título PK, Año PK, Duración, Tipo, Estudio FK)
- Estudios(Estudio PK, Dirección Estudio)
- Reparto(Título PK/FK, Año PK/FK, Actor PK, Rol)
-->

---

📌 **4. Tienda de Abarrotes — Inventario y Proveedores**

Una tienda de abarrotes maneja su inventario combinando información de
departamentos, productos, proveedores y precios en una sola tabla.

| Departamento | Código Producto | Producto                     | Pasillo | Proveedor          | Costo | Margen | Precio | Unidad |
|--------------|-----------------|------------------------------|---------|--------------------|-------|--------|--------|--------|
| Frutas       | 4081            | Plátano                      | 1       | Frutas del Campo   | 0.20  | 75%    | 0.35   | kg     |
| Frutas       | 4027            | Toronja                      | 1       | Frutas del Campo   | 0.45  | 100%   | 0.90   | pza    |
| Frutas       | 4108            | Tomate saladette              | 1       | Verduras Express   | 1.89  | 5%     | 1.99   | kg     |
| Frutas       | 4851            | Apio                         | 1       | Frutas del Campo   | 1.00  | 100%   | 2.00   | pza    |
| Carnes       | 331100          | Alas de pollo                | 5       | Cárnicos MX        | 0.50  | 300%   | 1.50   | kg     |
| Carnes       | 331105          | Carne molida magra           | 5       | Cárnicos MX        | 0.60  | 400%   | 2.40   | kg     |
| Carnes       | 332110          | Pechuga deshuesada           | 5       | Cárnicos MX        | 2.50  | 100%   | 5.00   | kg     |
| Congelados   | 411100          | Jugo de naranja              | 6       | Jugos Don Pedro    | 0.25  | 400%   | 1.00   | pza    |
| Congelados   | 521101          | Jugo de manzana              | 6       | Jugos Don Pedro    | 0.25  | 400%   | 1.00   | pza    |
| Congelados   | 866503          | Helado de vainilla           | 6       | Helados Cremosos   | 2.50  | 100%   | 5.00   | pza    |
| Congelados   | 866504          | Helado de chocolate          | 6       | Helados Cremosos   | 2.50  | 100%   | 5.00   | pza    |

**Dependencias funcionales:**
- Código Producto → Producto, Departamento, Pasillo, Precio, Unidad, Proveedor,
  Costo, Margen  
- Departamento → Pasillo  
- Proveedor → (no determina nada por sí solo, un proveedor puede tener
  múltiples productos)  

> **Pista:** Identifica qué atributos dependen del producto y cuáles del
> departamento. ¿Hay dependencias transitivas?

<!--
RESPUESTA EJERCICIO 4:

Ya está en 1FN.

PK: {Código Producto}

Ya está en 2FN (la PK no es compuesta, no puede haber dependencias parciales).

Violaciones de 3FN (dependencias transitivas):
- Código Producto → Departamento → Pasillo
  (Pasillo depende de Departamento, no directamente del producto)

Descomposición a 3FN:
- Departamentos(Departamento PK, Pasillo)
- Productos(Código Producto PK, Producto, Departamento FK, Precio, Unidad,
  Proveedor, Costo, Margen)

Nota: Proveedor no genera dependencia transitiva porque no determina
funcionalmente otros atributos (un proveedor tiene múltiples productos con
diferentes costos y márgenes). Si se quisiera almacenar datos del proveedor
(dirección, teléfono, etc.) sí se separaría en otra tabla, pero con los
datos actuales no es necesario.
-->

---

📌 **5. Registro Escolar — Alumnos, Materias y Casas**

Una escuela registra a sus alumnos, la casa a la que pertenecen (sistema de
casas), las materias que cursan y sus calificaciones.

| Matrícula  | Nombre Alumno  | Dirección             | Casa    | Color Casa | Materia      | Costo Materia | Calificación | Profesor       | Depto. Profesor |
|------------|----------------|-----------------------|---------|------------|--------------|---------------|--------------|----------------|-----------------|
| 19594332X  | María García   | Av. Reforma 10        | Águilas | Rojo       | Inglés       | $500          | 8            | Prof. Martínez | Idiomas         |
| 19594332X  | María García   | Av. Reforma 10        | Águilas | Rojo       | Matemáticas  | $500          | 9            | Prof. López    | Ciencias        |
| 19594332X  | María García   | Av. Reforma 10        | Águilas | Rojo       | Informática  | $1000         | 8            | Prof. Ruiz     | Tecnología      |
| 20601245Y  | Carlos Méndez  | Calle Juárez 45       | Lobos   | Azul       | Matemáticas  | $500          | 7            | Prof. López    | Ciencias        |
| 20601245Y  | Carlos Méndez  | Calle Juárez 45       | Lobos   | Azul       | Informática  | $1000         | 9            | Prof. Ruiz     | Tecnología      |
| 20601245Y  | Carlos Méndez  | Calle Juárez 45       | Lobos   | Azul       | Historia     | $400          | 8            | Prof. Vega     | Humanidades     |
| 21705678Z  | Ana Torres     | Blvd. Hidalgo 88      | Águilas | Rojo       | Inglés       | $500          | 10           | Prof. Martínez | Idiomas         |
| 21705678Z  | Ana Torres     | Blvd. Hidalgo 88      | Águilas | Rojo       | Historia     | $400          | 7            | Prof. Vega     | Humanidades     |

**Dependencias funcionales:**
- Matrícula → Nombre Alumno, Dirección, Casa  
- Casa → Color Casa  
- Materia → Costo Materia, Profesor  
- Profesor → Depto. Profesor  
- {Matrícula, Materia} → Calificación  

> **Pista:** Este ejercicio tiene múltiples niveles de dependencias transitivas.
> Identifica la cadena: Materia → Profesor → Depto. Profesor.

<!--
RESPUESTA EJERCICIO 5:

Ya está en 1FN.

PK: {Matrícula, Materia}

Violaciones de 2FN (dependencias parciales):
- Matrícula → Nombre Alumno, Dirección, Casa (depende solo de parte de la PK)
- Materia → Costo Materia, Profesor (depende solo de parte de la PK)

Descomposición a 2FN:
- Alumnos(Matrícula PK, Nombre Alumno, Dirección, Casa)
- Materias(Materia PK, Costo Materia, Profesor)
- Inscripciones(Matrícula PK/FK, Materia PK/FK, Calificación)

Violaciones de 3FN (dependencias transitivas):
- En Alumnos: Casa → Color Casa (Matrícula → Casa → Color Casa)
- En Materias: Profesor → Depto. Profesor (Materia → Profesor → Depto. Profesor)

Descomposición a 3FN:
- Alumnos(Matrícula PK, Nombre Alumno, Dirección, Casa FK)
- Casas(Casa PK, Color Casa)
- Materias(Materia PK, Costo Materia, Profesor FK)
- Profesores(Profesor PK, Depto. Profesor)
- Inscripciones(Matrícula PK/FK, Materia PK/FK, Calificación)
-->

---

📌 **6. Galería de Arte — Ventas y Reventas**

Una galería de arte vende pinturas y puede recomprarlas y revenderlas. Un
artista puede tener múltiples obras. Una misma obra puede venderse varias veces.

| No. Cliente | Nombre Cliente    | Teléfono     | Dirección Cliente      | Ciudad     | No. Artista | Nombre Artista    | Título Obra                | Fecha Venta | Precio Venta | Técnica   |
|-------------|-------------------|--------------|------------------------|------------|-------------|-------------------|----------------------------|-------------|--------------|-----------|
| CL01        | Elena Ríos        | 555-867-5309 | Av. Insurgentes 123    | CDMX       | A03         | Carmen Montoya    | Risa con Dientes           | 2024-09-17  | 7000         | Óleo      |
| CL01        | Elena Ríos        | 555-867-5309 | Av. Insurgentes 123    | CDMX       | A15         | Diego Fuentes     | Mar hacia Esmeralda        | 2024-05-11  | 1800         | Acuarela  |
| CL01        | Elena Ríos        | 555-867-5309 | Av. Insurgentes 123    | CDMX       | A03         | Carmen Montoya    | En el Cine                 | 2025-02-14  | 5550         | Óleo      |
| CL02        | Roberto Salinas   | 555-234-8800 | Calle Madero 45        | Monterrey  | A15         | Diego Fuentes     | Mar hacia Esmeralda        | 2025-01-20  | 2200         | Acuarela  |
| CL02        | Roberto Salinas   | 555-234-8800 | Calle Madero 45        | Monterrey  | A08         | Laura Vásquez     | Amanecer en el Valle       | 2025-03-10  | 3400         | Mixta     |
| CL03        | Sofía Delgado     | 555-111-2233 | Blvd. Revolución 78    | Guadalajara| A03         | Carmen Montoya    | Risa con Dientes           | 2025-07-22  | 8500         | Óleo      |
| CL03        | Sofía Delgado     | 555-111-2233 | Blvd. Revolución 78    | Guadalajara| A15         | Diego Fuentes     | Paisaje Nocturno           | 2025-08-05  | 2000         | Acuarela  |

**Dependencias funcionales:**
- No. Cliente → Nombre Cliente, Teléfono, Dirección Cliente, Ciudad  
- No. Artista → Nombre Artista  
- {No. Artista, Título Obra} → Técnica  
- {No. Cliente, No. Artista, Título Obra, Fecha Venta} → Precio Venta  

> **Pista:** La clave primaria es bastante grande. Piensa en qué entidades
> puedes separar para reducir la redundancia. Nota que "Risa con Dientes" se
> vendió a dos clientes diferentes (la galería la recompró y revendió).

<!--
RESPUESTA EJERCICIO 6:

Ya está en 1FN.

PK: {No. Cliente, No. Artista, Título Obra, Fecha Venta}

Violaciones de 2FN (dependencias parciales):
- No. Cliente → Nombre Cliente, Teléfono, Dirección Cliente, Ciudad
- No. Artista → Nombre Artista
- {No. Artista, Título Obra} → Técnica

Descomposición a 2FN:
- Clientes(No. Cliente PK, Nombre Cliente, Teléfono, Dirección Cliente, Ciudad)
- Artistas(No. Artista PK, Nombre Artista)
- Obras(No. Artista PK/FK, Título Obra PK, Técnica)
- Ventas(No. Cliente PK/FK, No. Artista PK/FK, Título Obra PK/FK,
  Fecha Venta PK, Precio Venta)

3FN: No hay dependencias transitivas en las tablas resultantes.
El esquema ya está en 3FN.
-->

---

### **Entregables**

Para cada ejercicio entrega:
1. **Identificación de violaciones** — lista las violaciones de 1FN, 2FN y 3FN.
2. **Proceso de normalización** — muestra las tablas resultantes en cada paso
   (1FN → 2FN → 3FN).
3. **Esquema final en 3FN** — las tablas finales con sus claves primarias (PK)
   y claves foráneas (FK) claramente indicadas.
4. **Modelo en Pony Editor** — captura de pantalla del diagrama final modelado
   en [Pony Editor](https://editor.ponyorm.com/).

---

💡 **Consejos:**
- En cada paso, pregúntate: *¿todos los atributos no clave dependen de TODA la
  clave primaria?* (2FN) y *¿algún atributo no clave depende de otro atributo
  no clave?* (3FN).  
- En Pony Editor, usa las relaciones para representar las claves foráneas.  

---

**Fuentes de referencia:**  
- [Normalization Exercises - CS 374, JMU](https://w3.cs.jmu.edu/cs374/s24/labs/normalization/)  
- [Database Normalization Examples - COMP 440](https://mebrahimii.github.io/comp440-fall2020/lecture/week_13/DB%20Normalization%20Example.pdf)  
- [Ejercicio de Normalización - Universidad de Granada](https://ccia.ugr.es/~cdemesa/pbd/docs/ejercicio_normalizacion_propuesto.pdf)  
- [DATABASE DESIGN: Normalization Exercises](https://www.javaguicodexample.com/normalizationexerciseanswer.pdf)  
