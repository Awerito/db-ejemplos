# Tarea: Migraciones, reglas de negocio y vistas en un cine

- **Modalidad:** individual - para la casa
- **Inicio:** jueves 2026-06-11 - **Entrega:** jueves 2026-06-18
- **Puntaje total:** 100 puntos
- **Motor:** PostgreSQL
- **Temas:** Migraciones - Funciones y triggers PL/pgSQL - Vistas - Datos de
  prueba - Consultas SQL

**Entregas fuera de plazo no se consideran.**

Podrán y deberían consultar al cliente (profesor) cuando existan dudas,
pidiendo retroalimentación de avances, esto con la idea de formar la costumbre
de validar continuamente el desarrollo. Es mala señal no escuchar del
desarrollador despues de la primera reunión solo para ver que el dia de la
entrega aparecen con algo "a medio cocer"

---

## 🎬 Contexto

Un cine de barrio lo tiene todo en hojas de Excel: en una planilla anotan qué
película pasa en qué sala y a qué hora, y en otra van apuntando las entradas
vendidas con su butaca. Funcionó por un tiempo, pero ya aparecieron
los problemas de siempre: dos películas quedaron agendadas en la misma sala a la
misma hora, se vendió dos veces la butaca C12, se registró un RUT que no existe,
y al cierre nadie sabe cuánto recaudó cada función, cuáles son los horarios peak
ni qué tan llenas quedan las salas.

Te contratan para llevar ese caos a una base de datos **PostgreSQL** seria. Pero
hay una condición central: **todo tu trabajo vive en migraciones**. El dueño del
cine no quiere "una base que tú armaste a mano"; quiere poder tomar tus archivos,
aplicarlos en orden sobre una base vacía y quedar con **exactamente la misma base
de datos que tú**: mismas tablas, mismas reglas, mismas vistas y los mismos datos
de prueba. Si algo no está en una migración, no existe.

---

## 🎯 Filosofía de la entrega

> **El entregable son las migraciones.** Si quien corrige aplica tus migraciones
> en orden sobre una base limpia, debe obtener la misma base de datos que tú: el
> mismo esquema, las mismas reglas de negocio, las mismas vistas y los mismos
> datos. Nada se hace "por fuera".

La **única excepción** es el archivo `consultas.sql` con las respuestas a las
preguntas de la última sección: esas son consultas de lectura, no cambios al
esquema, así que no van como migración.

---

## 📋 Instrucciones

1. **Diseña tu modelo inicial.** Tú decides las tablas, claves primarias y claves
   foráneas que necesita el cine, y si quieres puedes bocetarlo primero en
   [Pony Editor](https://editor.ponyorm.com/). Ese modelo inicial es tu migración
   `000_modelo_inicial.sql`.

2. **Avanza por migraciones numeradas.** Cada cambio posterior va en su propio
   archivo `NNN_descripcion.sql` (`001`, `002`, …) dentro de una carpeta
   `migraciones/`. La numeración refleja el orden en que resolviste los problemas:
   partes con un modelo simple y lo vas corrigiendo.

3. **Usa estas plantillas de migración.** Siguen el mismo formato del
   [modelo de la clase 12](https://github.com/Awerito/base-de-datos-apuntes/tree/master/12-consultas-sql/model).
   Tu `000` crea el schema, la tabla de control `schema_migrations` y tu modelo
   base; cada migración posterior fija el `search_path`, hace **un** cambio y
   registra su versión al final.

   El `000-template`, la migración inicial:

   ```sql
   -- ************************************************************
   -- 000-modelo-inicial.sql
   --
   -- Migración inicial: crea el schema, la tabla de control de
   -- migraciones y el modelo base del cine.
   -- ************************************************************

   -- Crea el schema y trabaja dentro de él.
   CREATE SCHEMA IF NOT EXISTS cine;
   SET search_path TO cine;

   -- Tabla de control de versiones del esquema.
   CREATE TABLE schema_migrations (
       version TEXT PRIMARY KEY,
       applied_at TIMESTAMP DEFAULT now()
   );

   -- ... aquí van las tablas de tu modelo ...

   -- Registro de versión si la migración corre con éxito.
   INSERT INTO schema_migrations (version) VALUES ('000-modelo-inicial');
   ```

   El `xxx-template`, para cada migración siguiente:

   ```sql
   -- ************************************************************
   -- 00X-descripcion-corta.sql
   --
   -- Qué problema resuelve esta migración (una sola cosa).
   -- ************************************************************

   SET search_path TO cine;

   -- ... el cambio de esta migración ...

   INSERT INTO schema_migrations (version) VALUES ('00X-descripcion-corta');
   ```

4. **Ejecuta cada migración con commit manual.** Aplica una migración a la vez
   con el auto-commit desactivado en tu cliente: si algo falla, haz `ROLLBACK` y
   la base queda como antes; si todo salió bien, haz `COMMIT` y confirmas el
   cambio junto con su registro en `schema_migrations`. Así ambos se aplican
   juntos, o no se aplica ninguno.

5. **Una migración = un cambio.** No mezcles cinco cosas distintas en un archivo,
   y **no edites una migración ya aplicada**: si te equivocaste, lo corriges con
   una migración nueva.

6. **Protege cada objeto que crees.** Como todo vive en migraciones y se aplican
   sobre la base, cada sentencia de creación debe llevar su salvaguarda para no
   fallar al correrla: `CREATE TABLE IF NOT EXISTS`, `CREATE OR REPLACE VIEW`,
   `CREATE OR REPLACE FUNCTION`, y un `DROP ... IF EXISTS` antes de recrear cuando
   corresponda.

---

## ✅ Requisitos

### 1. Migraciones incrementales

Tu `000_modelo_inicial.sql` con el modelo de partida, más las migraciones
siguientes que vayan corrigiendo y completando el esquema hasta soportar todo lo
que pide esta tarea.

Como buena práctica de diseño, nombra la clave primaria de cada tabla como
`<nombre_tabla>_id` y reusa ese mismo nombre en las claves foráneas. Así puedes
unir las tablas con `JOIN ... USING (<nombre_tabla>_id)`.

### 2. Reglas de negocio con funciones y triggers

Implementa las reglas del negocio mediante **funciones SQL / PL-pgSQL y
triggers**, cada una en su migración. Deben ir más allá de un simple
`columna > 0`. Como mínimo:

- **Validación de RUT**: el RUT de un cliente debe ser válido según su dígito
  verificador (módulo 11). No basta con que "sea un número".
- **Validación de email**: el email debe tener un formato válido
  (`algo@dominio.tld`).
- **Sin choques de funciones**: una misma sala no puede tener dos funciones que
  se solapen en el tiempo, considerando la duración de cada película.
- **Sin sobreventa**: no se pueden vender más entradas que la capacidad de la
  sala de esa función.
- **Asiento único**: el mismo asiento no se puede vender dos veces en una misma
  función.

### 3. Vistas para la aplicación

Estas vistas son lo que la aplicación del cine mostrará por encima de la base.
Crea cada una en su propia migración y calcula exactamente lo que se pide:

- **`vista_llenado_sala`**: una fila por sala, con su nombre, su capacidad y una
  columna `porcentaje_llenado`. Ese porcentaje compara las entradas vendidas en
  todas sus funciones contra el total de asientos ofrecidos, que es la capacidad
  multiplicada por el número de funciones de esa sala.
- **`vista_ventas_funcion`**: una fila por función, con su identificación
  (película, sala, fecha y hora), el monto total vendido en ella y una columna con
  el porcentaje que ese monto representa sobre la recaudación total del día.
- **`vista_disponibilidad_funcion`**: la vista que usa la app al momento de
  vender una entrada. Por cada función muestra cuántos asientos hay disponibles,
  cuáles están disponibles con sus identificadores, cuántas entradas se han
  vendido y cuánto se ha recaudado.

Para listar *cuáles* asientos quedan libres no basta con un número de capacidad:
tu modelo debe permitir saber qué asientos existen en cada sala y cuáles ya están
tomados en cada función.

### 4. Datos de prueba

Una o más migraciones que **rellenen la base con datos**: varias películas,
salas, funciones, clientes y entradas. Estos datos deben ser **fijos y
determinísticos**, sin valores aleatorios, para que tus consultas de la siguiente
sección den **el mismo resultado** cuando se corrija. Y deben respetar todas las
reglas de negocio que definiste.

### 5. Consultas

Responde las siguientes preguntas con una consulta SQL cada una, en el archivo
`consultas.sql`, que no es una migración. Numera cada respuesta con un comentario.

1. **Top 3 películas por recaudación total.**
2. **¿Qué función tuvo el mayor porcentaje de ocupación, es decir más entradas
   vendidas respecto a la capacidad de su sala?**
3. **¿Qué clientes compraron entradas en 3 o más funciones distintas?**
4. **Recaudación por sala y por día.**
5. **¿Qué películas no tienen ninguna entrada vendida?**
6. **¿Cuál es el horario de inicio con más entradas vendidas** (la franja peak)?

---

## 📊 Rúbrica

| Sección                                                                                    | Puntos |
|-------------------------------------------------------------------------                   |-------:|
| 1. Modelo inicial coherente y migraciones bien numeradas/atómicas                          | 20     |
| 2. Registro correcto en `schema_migrations` y reproducibilidad                             | 10     |
| 3. Reglas de negocio con funciones/triggers: RUT, email, solapamiento, capacidad y asiento | 30     |
| 4. Las 3 vistas pedidas: llenado de sala, ventas por función y disponibilidad              | 15     |
| 5. Datos de prueba determinísticos y que respetan las reglas                               | 10     |
| 6. Las 6 consultas correctas                                                               | 15     |
| **Total**                                                                                  | **100**|

---

## 📦 Entrega

Entrega una carpeta con esta estructura:

```
<nombre-completo>/
├── migraciones/
│   ├── 000_modelo_inicial.sql
│   ├── 001_*.sql
│   ├── 002_*.sql
│   └── ...               reglas de negocio, vistas y seed, cada una numerada
├── consultas.sql
└── link.txt link al modelo de ponyorm (opcional)
```

Quien corrija debe poder, sobre una base PostgreSQL vacía, aplicar tus
migraciones **en orden** desde `000` en adelante y obtener la misma base que tú;
luego ejecutar `consultas.sql` y obtener tus mismos resultados.
