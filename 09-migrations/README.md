## **Actividad en Clase: Práctica de Migraciones SQL**

⏳ **Duración total: 120 minutos**
🎯 **Objetivo:** Aplicar conocimientos sobre **migraciones SQL** mediante la
planificación, ejecución y control manual de cambios estructurales sobre un
esquema relacional existente.

---

1. **Trabajo individual**
2. **Ejercicio práctico:** Usar el esquema de base de datos con las tablas
   `cliente`, `producto`, `pedido`, `producto_pedido` y `categoria` para
realizar migraciones que modifican su estructura.
3. Para iniciar, utiliza el modelo en
   [pony](https://editor.ponyorm.com/user/Awerito/ComprasOnline/designer) y
agrega datos de prueba.

---

### **📋 Instrucciones Generales**

1. Crea una carpeta `migraciones/` y dentro de ella crea archivos SQL numerados
   (`001`, `002`, …).
2. Aplica cada archivo uno por uno en tu motor de base de datos.
3. Después de aplicar una migración, regístrala en la tabla `schema_migrations`:

```sql
INSERT INTO schema_migrations (version) VALUES ('XXX_nombre_migracion');
```

4. Si una migración causa problemas, crea su versión de rollback en otro
   archivo (`XXX_rollback_nombre.sql`).

---

### **🔧 Migraciones a Realizar**

✅ Cada migración debe estar en un archivo separado.

#### `001_add_telefono_to_cliente.sql`

Agrega una columna `telefono` a la tabla `cliente`.

```sql
ALTER TABLE cliente ADD COLUMN telefono TEXT;
```

---

#### `002_change_precio_to_decimal.sql`

Modifica el tipo de `precio_unidad` en `producto` para que acepte decimales.

```sql
ALTER TABLE producto ALTER COLUMN precio_unidad TYPE DECIMAL(10,2);
```

---

#### `003_add_unique_constraint_to_nombre_producto.sql`

Evita productos duplicados en nombre.

```sql
ALTER TABLE producto ADD CONSTRAINT nombre_unico UNIQUE(nombre);
```

---

#### `004_create_descuento_table.sql`

Crea la tabla `descuento` asociada a `producto` (recuerda que un descuento
puede tomar valores entre 0 y 100).
```sql
CREATE TABLE descuento (
  id SERIAL PRIMARY KEY,
  producto_id INTEGER NOT NULL,
  porcentaje INTEGER NOT NULL CHECK (porcentaje >= 0 AND porcentaje <= 100),
  activo BOOLEAN DEFAULT true,
  FOREIGN KEY (producto_id) REFERENCES producto(id) ON DELETE CASCADE
);
```

---

#### `005_rename_column_producto_pedido.sql`

Renombra las columnas `productos` → `producto_id` y `pedidos` → `pedido_id`.

```sql
ALTER TABLE producto_pedido RENAME COLUMN productos TO producto_id;
ALTER TABLE producto_pedido RENAME COLUMN pedidos TO pedido_id;
```

---

#### `006_add_estado_to_pedido.sql`

Agrega columna `estado` a `pedido`, con valor por defecto `'pendiente'`.

```sql
ALTER TABLE pedido ADD COLUMN estado TEXT DEFAULT 'pendiente';
```

---

#### `007_create_index_producto_nombre_precio.sql`

Crea índice compuesto en `producto(nombre, precio_unidad)`.

```sql
CREATE INDEX idx_producto_nombre_precio ON producto(nombre, precio_unidad);
```

---

#### `008_add_total_productos_to_pedido.sql`

Agrega columna `total_productos` a `pedido`.

```sql
ALTER TABLE pedido ADD COLUMN total_productos INTEGER DEFAULT 0;
```

---

#### `009_separate_direccion_cliente_to_street_and_city.sql`

Separa la columna `direccion` de `cliente` en `calle` y `ciudad`, la separación es por coma `,`.

```sql
ALTER TABLE cliente
  ADD COLUMN calle TEXT,
  ADD COLUMN ciudad TEXT;

UPDATE cliente
SET calle = split_part(direccion, ',', 1),
    ciudad = split_part(direccion, ',', 2);

ALTER TABLE cliente DROP COLUMN direccion;
```

---

### **🧪 Verificación**

Después de cada migración:

* Verifica que los cambios se reflejen en el esquema.
* Inserta datos de prueba si es necesario.
* Usa herramienta favorita para revisar restricciones, índices y columnas.

---

### **↩️ Rollbacks Opcionales**

✅ Crea versiones *rollback* para las migraciones que alteren o eliminen datos.
Ejemplo:

#### `002_rollback_change_precio_to_decimal.sql`

```sql
ALTER TABLE producto ALTER COLUMN precio_unidad TYPE INTEGER USING precio_unidad::INTEGER;
```

---

💬 **Reflexión final**

* ¿Qué ventajas te ofrece separar cada cambio en un archivo individual?
* ¿Qué errores podrían surgir si haces migraciones sin control ni rollback?
* ¿Qué consideraciones tomarías antes de eliminar columnas o tablas en
producción?
