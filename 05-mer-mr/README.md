## **Actividad: Conversión de MER a MR - Reglas de Cardinalidad**

⏳ **Duración estimada: 60 minutos**  
🎯 **Objetivo:** Aplicar las reglas de cardinalidad (1:1, 1:N, N:N) para
convertir modelos entidad-relación en diagramas del modelo relacional,
identificando tablas, claves primarias, claves foráneas y tablas intermedias
donde corresponda.

---

### **Instrucciones:**
1. **Identifica las entidades y sus atributos** a partir del enunciado.
2. **Determina la cardinalidad** de cada relación (1:1, 1:N o N:N).
3. **Dibuja el diagrama relacional** aplicando las reglas:  
   - **1:1** → Clave foránea con restricción UNIQUE en una de las tablas.  
   - **1:N** → Clave foránea en la tabla del lado "N".  
   - **N:N** → Tabla intermedia con clave primaria compuesta por las claves
     foráneas de ambas entidades.
4. En el diagrama, indica claramente: **PK**, **FK**, y las relaciones entre
   tablas.
5. **Dibuja un diagrama ER** utilizando un programa como **draw.io**,
   **Lucidchart** o cualquier otra herramienta de modelado.  

---

### **Enunciados de Problema**

📌 **1. Liga de Fútbol Profesional**
*"La liga de fútbol profesional desea crear una base de datos para guardar la
información de los partidos que se juegan. De cada jugador se almacena: código,
nombre, fecha de nacimiento y posición de juego. De cada equipo se almacena:
código, nombre, nombre del estadio, capacidad del estadio, año de fundación y
ciudad. Un jugador pertenece a un solo equipo, pero un equipo tiene muchos
jugadores. Los partidos tienen: código, fecha, goles locales y goles
visitantes. De cada gol se registra: minuto y descripción. Un partido tiene
varios goles, y un jugador puede anotar varios goles en un mismo partido. Cada
equipo tiene un único presidente y cada presidente dirige un solo equipo. Del
presidente se almacena: DNI, nombre, apellidos, fecha de nacimiento y año de
elección."*

📌 **2. Concesionario de Coches**
*"A un concesionario de coches llegan clientes para comprar automóviles. De
cada coche se almacena: matrícula, marca, modelo, color y precio de venta. De
cada cliente se almacena: NIF, nombre, dirección, ciudad, teléfono y un código
interno. Un cliente puede comprar varios coches, pero cada coche es comprado
por un solo cliente. Los coches que vende el concesionario pueden ser nuevos o
usados. De los nuevos se almacena la cantidad de unidades, y de los usados los
kilómetros recorridos. El concesionario dispone de un taller donde los
mecánicos reparan los coches. De cada mecánico se almacena: DNI, nombre,
apellidos, fecha de contratación y salario. Un mecánico repara varios coches y
un coche puede ser reparado por varios mecánicos. Se debe registrar la fecha de
reparación y las horas empleadas."*

📌 **3. Organización Empresarial**
*"Una empresa necesita organizar la información referente a su estructura
interna. La empresa está organizada en departamentos, de los cuales se
almacena: código, nombre y presupuesto anual. Los departamentos se ubican en
centros de trabajo, de los cuales se almacena: código, nombre, población y
dirección. Cada departamento está en un solo centro de trabajo, pero un centro
puede contener varios departamentos. De los empleados se almacena: teléfono,
fecha de contratación, NIF, nombre, cantidad de hijos y salario. De los hijos
de cada empleado se almacena: código, nombre y fecha de nacimiento. Un empleado
está asignado a un solo departamento, pero un departamento tiene varios
empleados. Los empleados pueden tener varias habilidades, y una misma habilidad
puede ser compartida por varios empleados. De cada habilidad se almacena:
código y descripción. Los centros de trabajo son dirigidos por empleados; un
empleado puede dirigir varios centros."*

📌 **4. Agencia de Seguros**
*"Una agencia de seguros necesita una base de datos para llevar el control de
accidentes y multas. De los propietarios de vehículos se almacena: nombre,
apellidos, dirección, población, teléfono y DNI. De los vehículos se almacena:
matrícula, marca y modelo. Una persona puede ser propietaria de varios
vehículos, y un vehículo puede pertenecer a varias personas. Los accidentes
tienen: número de referencia (secuencial), fecha, lugar y hora. En un accidente
participan varias personas y varios vehículos. Las multas tienen: número de
referencia (secuencial), fecha, hora, lugar de la infracción e importe. Una
multa se aplica a un solo conductor, y una multa involucra un solo vehículo."*

📌 **5. Empresa de Transporte**
*"Una empresa de transporte necesita gestionar la distribución de paquetes por
toda la región. De los conductores se almacena: DNI, nombre, teléfono,
dirección, salario y población. De los paquetes se almacena: código,
descripción, destinatario y dirección del destinatario. Un conductor distribuye
muchos paquetes, pero cada paquete es distribuido por un solo conductor. Existen
provincias con código y nombre. Cada paquete llega a una sola provincia, pero
una provincia recibe muchos paquetes. De los camiones se almacena: matrícula,
modelo, tipo y potencia. Un conductor puede manejar distintos camiones en
diferentes momentos, y un camión puede ser operado por varios conductores."*

📌 **6. Gestión de Proyectos**
*"Una empresa desea almacenar la información generada en sus proyectos. De cada
proyecto se almacena: código, descripción, monto, fecha de inicio y fecha de
fin. De cada cliente se almacena: código, teléfono, dirección y nombre de
empresa. Un cliente puede realizar varios proyectos, pero cada proyecto
pertenece a un solo cliente. De los colaboradores se almacena: NIF, nombre,
dirección, teléfono, banco y número de cuenta. Varios colaboradores participan
en un proyecto, y un colaborador puede participar en varios proyectos. Los
pagos tienen: número, concepto, monto y fecha. Cada pago tiene un tipo de pago
asociado (código y descripción). Un tipo de pago puede aplicarse a varios
pagos."*

---

### **Fuentes**
Ejercicios adaptados de:
- [Modelo ERR - Ejercicios Resueltos (bdalfonso.blogspot.com)](http://bdalfonso.blogspot.com/2013/06/modelo-err-ejercicios-resueltos.html)
- [Ejercicios Entidad Relación 11 al 22 (karlajaneth.wordpress.com)](https://karlajaneth.wordpress.com/2010/03/20/ejercicios-entidad-relacion-11-al-22/)
