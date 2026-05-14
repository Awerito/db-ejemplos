## **Actividad en Clase: Traducción SQL ↔ Álgebra Relacional**

⏳ **Duración total: 60 minutos**  
🎯 **Objetivo:** Practicar la traducción **bidireccional** entre SQL y
álgebra relacional. Dada una consulta SQL, escribir la expresión equivalente
$\sigma, \pi, \bowtie, \cup, \cap, -, \rho$; y a la inversa, dada una
expresión de álgebra, escribir el `SELECT` equivalente.

---

1. **Trabajo individual o en parejas.**
2. **Pre-requisito:** clase
   [10 — Álgebra y Cálculo Relacional](../10-algebra-calculo-relacional/).
3. **Esquema de trabajo** (agencia de viajes — set de ejercicios resueltos
   de [exploredatabase.com](https://www.exploredatabase.com/2020/04/relational-algebra-in-database-solved-exercise.html)):

```
passenger(pid, pname, pgender, pcity)
agency(aid, aname, acity)
flight(fid, fdate, time, src, dest)
booking(pid, aid, fid, fdate)
```

con `booking.pid → passenger.pid`, `booking.aid → agency.aid`,
`booking.fid → flight.fid`.

4. **Configuración:** crear las 4 tablas con sus PK/FK en PostgreSQL e
   insertar datos mínimos que cubran los casos de cada ejercicio
   (pasajeros con y sin reservas, agencias en distintas ciudades, vuelos
   en varias fechas, una agencia llamada `'Jet'`).

---

### **Instrucciones**

- En la **Sección 1** entregas la expresión de álgebra equivalente a cada SQL.
- En la **Sección 2** entregas el SQL equivalente a cada expresión.
- Verifica cada SQL ejecutándolo contra tu instancia.

---

### **1. SQL → Álgebra Relacional**

Para cada consulta SQL, escribe la expresión equivalente en notación
$\sigma, \pi, \bowtie, \cup, \cap, -$.

1. Vuelos cuyo destino es *New Delhi*.

```sql
SELECT *
FROM flight
WHERE dest = 'New Delhi';
```

<!--
RA: σ_{dest='New Delhi'}(flight)
-->

2. Nombres de pasajeros con al menos una reserva.

```sql
SELECT DISTINCT p.pname
FROM passenger p
JOIN booking b ON b.pid = p.pid;
```

<!--
RA: π_{p.pname}(
       ρ_{p}(passenger) ⋈_{p.pid = b.pid} ρ_{b}(booking)
    )
Nota: los alias `p` y `b` del SQL se modelan con ρ aunque la consulta no
los necesite estrictamente (no hay autoreferencia). Forma equivalente sin
ρ usando join natural:
    π_{pname}( passenger ⋈ booking )
-->

3. Vuelos que operan **a las 16:00 tanto el 2020-12-01 como el 2020-12-02**.

```sql
SELECT *
FROM flight
WHERE fdate = DATE '2020-12-01' AND time = '16:00'
INTERSECT
SELECT *
FROM flight
WHERE fdate = DATE '2020-12-02' AND time = '16:00';
```

<!--
RA: σ_{fdate='2020-12-01' ∧ time='16:00'}(flight)
    ∩ σ_{fdate='2020-12-02' ∧ time='16:00'}(flight)
-->

4. Pasajeros **sin** reservas (pid y nombre).

```sql
SELECT p.pid, p.pname
FROM passenger p
WHERE p.pid NOT IN (SELECT pid FROM booking);
```

<!--
RA: π_{p.pid, p.pname}(
       ρ_{p}(passenger) ⋈_{p.pid = d.pid} ρ_{d}(
          π_{pid}(passenger) − π_{pid}(booking)
       )
    )
Nota: el alias `p` del SQL se modela con ρ_{p}; el subresultado de la
diferencia se renombra con ρ_{d} para escribir la condición de join sin
ambigüedad. Forma equivalente compacta sin ρ:
    π_{pid,pname}(
       ( π_{pid}(passenger) − π_{pid}(booking) ) ⋈ passenger
    )
-->

5. Pasajeros **masculinos** asociados a la agencia `'Jet'`.

```sql
SELECT DISTINCT p.pid, p.pname, p.pcity
FROM passenger p
JOIN booking b ON b.pid = p.pid
JOIN agency  a ON a.aid = b.aid
WHERE p.pgender = 'Male' AND a.aname = 'Jet';
```

<!--
RA: π_{p.pid, p.pname, p.pcity}(
       σ_{p.pgender='Male' ∧ a.aname='Jet'}(
          ρ_{p}(passenger) ⋈_{p.pid = b.pid} ρ_{b}(booking)
                                            ⋈_{b.aid = a.aid} ρ_{a}(agency)
       )
    )
Los alias `p, b, a` del SQL se modelan con ρ. Forma compacta sin ρ
(usando join natural sobre los atributos comunes):
    π_{pid,pname,pcity}(
       σ_{pgender='Male' ∧ aname='Jet'}( passenger ⋈ booking ⋈ agency )
    )
-->

---

### **2. Álgebra Relacional → SQL**

Para cada expresión, escribe la consulta SQL equivalente.

1. $\sigma_{src='Chennai' \,\wedge\, dest='New Delhi'}(flight)$

<!--
SQL:
  SELECT *
  FROM flight
  WHERE src = 'Chennai' AND dest = 'New Delhi';
-->

2. $\pi_{fid}\bigl(\sigma_{pid=123}(booking) \bowtie \sigma_{dest='Chennai'}(flight)\bigr)$

<!--
SQL:
  SELECT DISTINCT b.fid
  FROM booking b
  JOIN flight  f ON f.fid = b.fid
  WHERE b.pid = 123
    AND f.dest = 'Chennai';
-->

3. $\pi_{aname}\bigl(agency \bowtie_{agency.acity = passenger.pcity} \sigma_{pid=123}(passenger)\bigr)$

   ("agencias que están en la misma ciudad que el pasajero 123")

<!--
SQL:
  SELECT DISTINCT a.aname
  FROM agency a
  JOIN passenger p ON a.acity = p.pcity
  WHERE p.pid = 123;
-->

4. $\bigl(\sigma_{fdate='2020-12-01' \wedge time='16:00'}(flight)\bigr)\;\cup\;\bigl(\sigma_{fdate='2020-12-02' \wedge time='16:00'}(flight)\bigr)$

<!--
SQL:
  SELECT * FROM flight WHERE fdate = DATE '2020-12-01' AND time = '16:00'
  UNION
  SELECT * FROM flight WHERE fdate = DATE '2020-12-02' AND time = '16:00';
-->

5. $\pi_{aname}(agency)\;-\;\pi_{aname}\bigl(agency \bowtie \sigma_{pid=123}(booking)\bigr)$

    ("agencias en las que el pasajero 123 **no** tiene reservas")

<!--
SQL (EXCEPT):
  SELECT aname FROM agency
  EXCEPT
  SELECT a.aname
  FROM agency a
  JOIN booking b ON b.aid = a.aid
  WHERE b.pid = 123;

SQL (NOT EXISTS):
  SELECT a.aname
  FROM agency a
  WHERE NOT EXISTS (
    SELECT 1 FROM booking b
    WHERE b.aid = a.aid AND b.pid = 123
  );
-->

---

### **Entregable**

Un archivo `traducciones.md` (o `traducciones.sql` con bloques de
comentario) con los 10 ejercicios y sus traducciones. Las consultas SQL
deben ejecutarse sin error sobre tu instancia con datos de prueba.

---

💬 **Reflexión final**

- ¿Qué información se **pierde** al traducir SQL a álgebra relacional
  clásica (orden con `ORDER BY`, duplicados con `ALL`, `GROUP BY`,
  agregados, `NULL`)?
- ¿Qué patrón de SQL traduce naturalmente a $\sigma$, cuál a $\pi$, cuál
  a $\bowtie$, y cuál exige diferencia $-$ o intersección $\cap$?
- En la traducción **álgebra → SQL**, ¿cuándo conviene `JOIN ... ON`,
  cuándo subconsulta `IN`, y cuándo `EXCEPT`/`NOT EXISTS`?

---

### **Fuentes**

- [Explore Database — *Relational algebra solved exercises*](https://www.exploredatabase.com/2020/04/relational-algebra-in-database-solved-exercise.html)
  (esquema passenger/agency/flight/booking).
- [Stony Brook CSE 532 — *Relational Algebra and SQL* (cap. 5)](https://www3.cs.stonybrook.edu/~kifer/Courses/cse532/slides/ch5.pdf).
- [ULB INFO-H-417 — *Translation from SQL into the relational algebra*](https://cs.ulb.ac.be/public/_media/teaching/infoh417/01_-_sql2alg-sol-slides.pdf).
- [UC Berkeley CS 186 — *Relational Algebra notes*](https://cs186berkeley.net/sp21/resources/static/notes/n05-RelAlg.pdf).
