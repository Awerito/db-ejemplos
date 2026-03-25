## **Proyecto Integrador de Base de Datos: Gestión de Torneos de Tenis de Mesa con ELO**

⏳ **Duración estimada: Resto del curso**

👥 **Trabajo en grupo (3 o 4 personas)**

🎯 **Objetivo:** Diseñar e implementar un sistema de base de datos relacional
para gestionar torneos de tenis de mesa, con capacidad para gestionar
jugadores, asociaciones, torneos, partidos (individuales y dobles) y el cálculo
de ELO.

---

### **Descripción del Proyecto**

Diseñar modelo de gestión de torneos de tenis de mesa donde se pueda registrar
la información de los jugadores, sus asociaciones, y cómo participan en los
torneos. Los jugadores tienen un **ranking ELO** que se actualiza a lo largo
del tiempo según el resultado de los partidos. Los torneos deben organizarse en
fases de **grupos** (clasificación) y **eliminación directa**, y los partidos
pueden ser tanto **individuales** como **dobles**. Además, los jugadores pueden
tener **ciudades y países** diferentes de los de su asociación.

Un torneo puede tener **mesas** para repartir los partidos, y se deben asignar
**horarios** de inicio para los mismos. Los jugadores se inscriben en las
**categorías** que corresponden a su **edad** y **género**, y el torneo, una
vez cerradas las inscripciones, inicia el proceso de asignar los partidos a las
mesas. Este proceso comienza por la fase de grupos si es necesario o, en su
defecto, por la eliminación directa. Las llaves de eliminación directa pueden
tener un **Bye**, es decir, algunos jugadores avanzan automáticamente sin jugar
debido a que no se pueden emparejar todos los participantes (ej., si hay 7
jugadores, uno pasa directamente a la siguiente ronda).

El sistema de asignación de partidos debe gestionar tanto jugadores (en
partidos individuales) como equipos (en partidos de dobles). Para cada partido,
debe asignarse una mesa específica y un horario (fecha y hora) en la que se
jugará.

El sistema debe ser capaz de gestionar:

* Información de los **jugadores** (nombre, ciudad, país, asociación, ELO,
historial de partidos).
* Información de las **asociaciones** (nombre, ciudad, país).
* Información sobre los **torneos** (nombre, fecha, mesas disponibles,
categorías).
* **Categorías** de los torneos (por ejemplo: varón single 15, parejas mixto
17, etc.).
* **Partidos** (tanto individuales como dobles), con resultados, asignación de
mesas y horarios, y actualizaciones del ELO de los jugadores.
* **Equipos de dobles** en los partidos de esta modalidad.
* **Fase de grupos** y **eliminación directa** con asignación de partidos a
mesas.
* Gestión de **Bye** en las llaves de eliminación directa.
* Evolución del **ELO** de los jugadores a lo largo de los torneos.

---

### **📋 Instrucciones Generales**

1. **Trabajo en grupo (3 o 4 personas)**.
2. **Diseño de la base de datos**: Crear un esquema de base de datos usando
   **SQLAlchemy** y **Alembic** para las migraciones, que permita almacenar la
información de jugadores, asociaciones, torneos, partidos y el ELO de los
jugadores.
3. **Consultas**: Implementar funciones que permitan consultar la base de datos
   de acuerdo con los siguientes requerimientos utilizando **SQLAlchemy ORM**.
4. **Datos de prueba**: Incluir datos de prueba en la base de datos, con al
   menos 10 jugadores, 2 torneos y 3 partidos de ejemplo.
5. **Modelo en PonyORM**: Crear un modelo visual en
   [PonyORM](https://editor.ponyorm.com/) y entregar el link al diagrama
público.

---

### **🔧 Requisitos del Proyecto**

#### **1. Diagrama del Modelo en PonyORM**

* Crear el modelo en [PonyORM Editor](https://editor.ponyorm.com/) y entregar
el **enlace público** al diagrama del modelo.

#### **2. Repositorio del Proyecto**

* El repositorio debe contener:

  * El modelo implementado con **SQLAlchemy**.
  * Las migraciones de **Alembic** para la creación de las tablas y cualquier
  cambio posterior.
  * Un script `seed.py` que inserte datos de prueba en la base de datos.
  * Un archivo **queries.py** con las funciones que utilicen SQLAlchemy ORM
  para realizar las consultas solicitadas.

---

### **📋 Consultas Esperadas**

Las consultas deben ser implementadas usando **SQLAlchemy ORM**. Algunas de las
consultas requeridas son:

1. Obtener el **ranking de jugadores** por puntuación ELO.
2. Consultar el **historial de partidos** (individuales y dobles) de un jugador.
3. Ver los **resultados de un torneo**, incluyendo la fase de grupos y la
   eliminación directa.
4. Consultar la **evolución del ELO** de un jugador a través de los partidos
   jugados.
5. Obtener los **jugadores de una asociación** específica.
6. Buscar jugadores por **país** o **ciudad** de origen.

---

### **📦 Entregables**

1. **Link al diagrama** en [PonyORM Editor](https://editor.ponyorm.com/).
2. **Link al repositorio** que debe contener:

   * El modelo de base de datos implementado con **SQLAlchemy**.
   * Las migraciones con **Alembic**.
   * El script de **carga de datos de prueba** (`seed.py`).
   * Las funciones de consulta implementadas en un archivo **queries.py**.

---

### **🔧 Instrucciones de Ejecución**

1. **Aplicar las migraciones** para inicializar el esquema de la base de datos.
2. **Ejecutar el script de carga de datos** para insertar jugadores,
   asociaciones, torneos y partidos de ejemplo.
3. **Probar las funciones de consulta** para verificar que los resultados son
   correctos.

---

### **🧪 Verificación Antes de Entregar**

* Verifica que las **migraciones** se apliquen correctamente.
* Asegúrate de que las **consultas ORM** devuelvan los resultados correctos con
los datos de prueba.
* Revisa la estructura del **modelo de base de datos** para asegurar que las
relaciones entre las tablas estén correctamente definidas.

---

### **Evaluación**

* **Diseño del modelo de base de datos**: Correcta implementación del esquema
con las relaciones adecuadas entre las entidades.
* **Migraciones**: Uso adecuado de Alembic para crear y modificar las tablas.
* **Consultas**: Las funciones de consulta deben ser correctas, eficientes y
utilizar SQLAlchemy ORM.
* **Código limpio y bien documentado**: El código debe estar bien estructurado,
con nombres descriptivos y comentarios donde sea necesario.
* **Datos de prueba**: Los datos deben ser suficientes para probar todas las
consultas de manera efectiva.
