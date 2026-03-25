## **Actividad en Clase: Auditoría del ELO con Triggers**

⏳ **Duración total: 60 minutos**

🎯 **Objetivo:** Aplicar conocimientos sobre **funciones y triggers en
PostgreSQL** para implementar una auditoría automática del puntaje ELO de los
jugadores al ganar o perder partidas.

---

1. **Trabajo individual**
2. **Ejercicio práctico:** Modelar desde cero y trabajar con un esquema mínimo
   de base de datos para torneos de tenis de mesa.
3. Crear funciones y triggers para auditar automáticamente cambios en el ELO de
   cada jugador.

---

### **📋 Instrucciones Generales**

#### **1. Modela las siguientes tablas:**

- `jugador`: Almacena información de los jugadores.
- `partido`: Registra los partidos jugados, incluyendo el ganador y el perdedor.
- `elo_historial`: Guarda el historial de cambios del ELO de cada jugador.

<!-- ```sql -->
<!-- CREATE TABLE jugador ( -->
<!--     id SERIAL PRIMARY KEY, -->
<!--     nombre TEXT NOT NULL, -->
<!--     elo INTEGER NOT NULL DEFAULT 1000 -->
<!-- ); -->
<!---->
<!-- CREATE TABLE partido ( -->
<!--     id SERIAL PRIMARY KEY, -->
<!--     jugador_ganador_id INTEGER REFERENCES jugador(id), -->
<!--     jugador_perdedor_id INTEGER REFERENCES jugador(id), -->
<!--     fecha TIMESTAMP DEFAULT now() -->
<!-- ); -->
<!---->
<!-- CREATE TABLE elo_historial ( -->
<!--     id SERIAL PRIMARY KEY, -->
<!--     jugador_id INTEGER REFERENCES jugador(id), -->
<!--     elo_anterior INTEGER, -->
<!--     elo_nuevo INTEGER, -->
<!--     cambio INTEGER, -->
<!--     motivo TEXT, -->
<!--     fecha TIMESTAMP DEFAULT now() -->
<!-- ); -->
<!-- ``` -->

> ⚠️ ¡Hoy modelan ustedes!

---

#### **2. Crea una función `auditar_elo()` que registre los cambios en el ELO:**

Esta función debe:

* Detectar si el valor del ELO cambió.
* Insertar un nuevo registro en `elo_historial` con el valor anterior, nuevo,
el cambio y un motivo.

<!-- ```sql -->
<!-- CREATE OR REPLACE FUNCTION auditar_elo() -->
<!-- RETURNS TRIGGER AS $$ -->
<!-- BEGIN -->
<!--     IF NEW.elo <> OLD.elo THEN -->
<!--         INSERT INTO elo_historial (jugador_id, elo_anterior, elo_nuevo, cambio, motivo) -->
<!--         VALUES ( -->
<!--             NEW.id, -->
<!--             OLD.elo, -->
<!--             NEW.elo, -->
<!--             NEW.elo - OLD.elo, -->
<!--             TG_ARGV[0] -->
<!--         ); -->
<!--     END IF; -->
<!--     RETURN NEW; -->
<!-- END; -->
<!-- $$ LANGUAGE plpgsql; -->
<!-- ``` -->

---

#### **3. Crea un trigger que active la auditoría:**

Este trigger debe activarse **después de actualizar** el ELO de un jugador

<!-- ```sql -->
<!-- CREATE TRIGGER trigger_auditar_elo -->
<!-- AFTER UPDATE OF elo ON jugador -->
<!-- FOR EACH ROW -->
<!-- EXECUTE FUNCTION auditar_elo('actualización ELO por partida'); -->
<!-- ``` -->

---

### **🧪 Simulación de partida**

1. Inserta jugadores.
2. Simula partidas con `INSERT INTO partido (...)`.
3. Actualiza el ELO del ganador y del perdedor con `UPDATE jugador SET elo = ...`.
4. Verifica que `elo_historial` haya registrado los cambios correctamente.

---

### **💬 Reflexión final**

* ¿Qué utilidad tiene usar `OLD` y `NEW` dentro de un trigger?
* ¿Qué diferencias ves entre usar una función con parámetros y una función tipo
trigger?
* ¿Qué errores podrían ocurrir si no controlas cuándo insertar en la auditoría?
