# Entregables por sección

## Formato de entrega

- Un **único repositorio** por alumno para todo el curso. No crees un repo por
  sección ni por tarea.
- Una **carpeta por sección** y, dentro, una **subcarpeta por tarea** numerada
  con ceros a la izquierda. Coloca cada entregable en su subcarpeta:
- Si cuenta con la **Tarea 01: Cine** puede **no** hacer una sección y se le
  asignará la nota correspondiente de la tarea a dicha seccion, ej: falta
  sección 1, se asigna la nota de la tarea a esa sección.

```
repo-github/
├── seccion-1-modelamiento/
│   ├── 01-modelo-entidad-relacion/
│   ├── 02-modelo-relacional/
│   ├── 03-mer-mr/
│   ├── 04-normalizacion/
│   └── 05-normalizacion-avanzada/
├── seccion-2-sql/
│   ├── 01-creacion-tablas/
│   ├── 02-crud/
│   ├── 03-crud-biblioteca/
│   ├── 04-migraciones/
│   └── 05-triggers-funciones/
└── seccion-3-algebra-relacional/
    ├── 01-algebra-calculo-relacional/
    ├── 02-traducciones/
    └── 03-traducciones-set-2/
```

## Sección 1: Modelamiento

- [ ] [Modelo Entidad-Relación](03-modelo-entidad-relacion/README.md): diagramas ER de los enunciados (entidades, atributos y relaciones).
- [ ] [Modelo Relacional](04-modelo-relacional/README.md): conversión a MR de al menos 6 de los 12 MER, con claves primarias y foráneas.
- [ ] [MER a MR con cardinalidad](05-mer-mr/README.md): diagramas relacionales de los 6 enunciados con PK, FK y tablas intermedias según 1:1, 1:N y N:N.
- [ ] [Normalización](06-normalizacion/README.md): las 8 relaciones normalizadas hasta 3FN.
- [ ] [Normalización avanzada](06-normalizacion/README-2.md): los 6 ejercicios normalizados hasta 3FN y modelados en Pony Editor.

## Sección 2: SQL

- [ ] [Creación de tablas (DDL)](07-sql-creacion-tablas/README.md): archivo `modelo.sql` con todas las sentencias `CREATE TABLE`.
- [ ] [CRUD y vistas](08-sql-crud/README.md): consultas `SELECT`, `UPDATE`, `DELETE` y la vista `vista_global_academica`.
- [ ] [CRUD biblioteca](09-sql-biblioteca/README.md): consultas `SELECT`, `UPDATE`, `DELETE` y la vista `vista_global_biblioteca`.
- [ ] [Migraciones](13-migrations/README.md): carpeta `migraciones/` con los archivos 001 a 009, sus rollbacks y los registros en `schema_migrations`.
- [ ] [Triggers y funciones](14-triggers-funciones/README.md): modelo de las tablas `jugador`, `partido` y `elo_historial`, la función `auditar_elo()` y el trigger de auditoría.

## Sección 3: Álgebra relacional

- [ ] [Álgebra y cálculo relacional](10-algebra-calculo-relacional/README.md): por cada ejercicio, expresión de álgebra relacional, cálculo relacional y consulta SQL sobre Sailors-Boats-Reserves.
- [ ] [Traducciones SQL y álgebra](11-sql-algebra-traducciones/README.md): archivo `traducciones.md` con los 10 ejercicios (5 SQL a álgebra y 5 álgebra a SQL).
- [ ] [Traducciones SQL y álgebra (Set 2)](11-sql-algebra-traducciones/README-2.md): los 40 ejercicios (20 SQL a álgebra y 20 álgebra a SQL) sobre los 4 mini-esquemas.
