## **Actividad en Clase: Traducción SQL ↔ Álgebra Relacional (Set 2)**

⏳ **Duración total: 90 minutos**  
🎯 **Objetivo:** Segundo set de práctica para traducir **bidireccional**
entre SQL y álgebra relacional ($\sigma, \pi, \bowtie, \cup, \cap, -, \rho$),
ahora sobre cuatro mini-esquemas clásicos de la literatura.

---

1. **Trabajo individual**
2. **Esquemas de trabajo:**

```
# A — Sailors–Boats–Reserves       (Ramakrishnan/Gehrke)
sailors(sid, sname, rating, age)
boats(bid, bname, color)
reserves(sid, bid, rdate)

# B — Suppliers–Parts–Catalog       (York, Bolzano)
suppliers(sid, sname, address)
parts(pid, pname, color)
catalog(sid, pid, cost)

# C — Employees–Supervises          (Toronto CSC343)
employees(number, name, age, salary)
supervises(boss, employee)

# D — Clients–Staff–Appointments    (Toronto CSC343)
clients(cid, name, phone)
staff(sid, name)
appointments(cid, adate, atime, service, sid)
```

con FKs evidentes (`reserves.sid → sailors.sid`, `reserves.bid → boats.bid`,
`catalog.sid → suppliers.sid`, `catalog.pid → parts.pid`,
`supervises.boss/employee → employees.number`,
`appointments.cid → clients.cid`, `appointments.sid → staff.sid`).

---

### **Instrucciones**

- En la **Sección 1** entregas la expresión de álgebra equivalente a cada SQL.
- En la **Sección 2** entregas el SQL equivalente a cada expresión.
- Verifica cada SQL ejecutándolo contra tu instancia.

---

### **1. SQL → Álgebra Relacional**

Para cada consulta SQL, escribe la expresión equivalente en notación
$\sigma, \pi, \bowtie, \cup, \cap, -$.

1. Colores de los botes reservados por *Albert*.

```sql
SELECT DISTINCT b.color
FROM sailors s
JOIN reserves r ON r.sid = s.sid
JOIN boats    b ON b.bid = r.bid
WHERE s.sname = 'Albert';
```

<!--
RA: π_{color}( σ_{sname='Albert'}(sailors) ⋈ reserves ⋈ boats )
-->

2. `sid` de marineros con rating ≥ 8 o que reservaron el bote 103.

```sql
SELECT sid FROM sailors WHERE rating >= 8
UNION
SELECT sid FROM reserves WHERE bid = 103;
```

<!--
RA: π_{sid}(σ_{rating≥8}(sailors)) ∪ π_{sid}(σ_{bid=103}(reserves))
-->

3. Nombres de marineros que no reservaron un bote rojo.

```sql
SELECT s.sname
FROM sailors s
WHERE s.sid NOT IN (
  SELECT r.sid FROM reserves r JOIN boats b ON b.bid = r.bid
  WHERE b.color = 'red'
);
```

<!--
RA: π_{sname}(sailors) − π_{sname}( sailors ⋈ reserves ⋈ σ_{color='red'}(boats) )
-->

4. `sid` de marineros con edad > 20 que no reservaron un bote rojo.

```sql
SELECT s.sid
FROM sailors s
WHERE s.age > 20
  AND s.sid NOT IN (
    SELECT r.sid FROM reserves r JOIN boats b ON b.bid = r.bid
    WHERE b.color = 'red'
  );
```

<!--
RA: π_{sid}(σ_{age>20}(sailors)) − π_{sid}( reserves ⋈ σ_{color='red'}(boats) )
-->

5. Nombres de marineros que reservaron al menos dos botes distintos.

```sql
SELECT DISTINCT s.sname
FROM sailors s
JOIN reserves r1 ON r1.sid = s.sid
JOIN reserves r2 ON r2.sid = s.sid
WHERE r1.bid <> r2.bid;
```

<!--
RA: π_{s.sname}(
       σ_{r1.bid ≠ r2.bid}(
          ρ_{s}(sailors) ⋈_{s.sid=r1.sid} ρ_{r1}(reserves)
                          ⋈_{s.sid=r2.sid} ρ_{r2}(reserves)
       )
    )
-->

6. Nombres de marineros que reservaron todos los botes.

```sql
SELECT s.sname
FROM sailors s
WHERE NOT EXISTS (
  SELECT 1 FROM boats b
  WHERE NOT EXISTS (
    SELECT 1 FROM reserves r WHERE r.sid = s.sid AND r.bid = b.bid
  )
);
```

<!--
RA: π_{sname}( sailors ⋈ ( π_{sid,bid}(reserves) ÷ π_{bid}(boats) ) )
-->

7. Nombres de marineros que reservaron todos los botes llamados *BigBoat*.

```sql
SELECT s.sname
FROM sailors s
WHERE NOT EXISTS (
  SELECT 1 FROM boats b
  WHERE b.bname = 'BigBoat'
    AND NOT EXISTS (
      SELECT 1 FROM reserves r WHERE r.sid = s.sid AND r.bid = b.bid
    )
);
```

<!--
RA: π_{sname}( sailors ⋈ ( π_{sid,bid}(reserves) ÷ π_{bid}(σ_{bname='BigBoat'}(boats)) ) )
-->

8. Nombres de proveedores que suministran alguna parte roja.

```sql
SELECT DISTINCT s.sname
FROM suppliers s
JOIN catalog c ON c.sid = s.sid
JOIN parts   p ON p.pid = c.pid
WHERE p.color = 'red';
```

<!--
RA: π_{sname}( σ_{color='red'}(parts) ⋈ catalog ⋈ suppliers )
-->

9. `sid` de proveedores que suministran alguna parte roja o verde.

```sql
SELECT DISTINCT c.sid
FROM catalog c JOIN parts p ON p.pid = c.pid
WHERE p.color IN ('red','green');
```

<!--
RA: π_{sid}( σ_{color='red' ∨ color='green'}(parts) ⋈ catalog )
-->

10. `sid` de proveedores que suministran una parte roja o están en *21 George Street*.

```sql
SELECT c.sid FROM catalog c JOIN parts p ON p.pid=c.pid WHERE p.color='red'
UNION
SELECT sid FROM suppliers WHERE address = '21 George Street';
```

<!--
RA: π_{sid}( σ_{color='red'}(parts) ⋈ catalog ) ∪ π_{sid}( σ_{address='21 George Street'}(suppliers) )
-->

11. `sid` de proveedores que suministran alguna parte roja y alguna verde.

```sql
SELECT sid FROM catalog c JOIN parts p ON p.pid=c.pid WHERE p.color='red'
INTERSECT
SELECT sid FROM catalog c JOIN parts p ON p.pid=c.pid WHERE p.color='green';
```

<!--
RA: π_{sid}(σ_{color='red'}(parts) ⋈ catalog) ∩ π_{sid}(σ_{color='green'}(parts) ⋈ catalog)
-->

12. Pares de `sid` tales que el primero cobra más que el segundo por la misma parte.

```sql
SELECT c1.sid AS sid_caro, c2.sid AS sid_barato
FROM catalog c1
JOIN catalog c2 ON c1.pid = c2.pid AND c1.cost > c2.cost;
```

<!--
RA: π_{c1.sid, c2.sid}(
       ρ_{c1}(catalog) ⋈_{c1.pid=c2.pid ∧ c1.cost>c2.cost} ρ_{c2}(catalog)
    )
-->

13. `sid` de proveedores que suministran solo partes rojas.

```sql
SELECT sid FROM suppliers
EXCEPT
SELECT c.sid FROM catalog c JOIN parts p ON p.pid=c.pid WHERE p.color <> 'red';
```

<!--
RA: π_{sid}(suppliers) − π_{sid}( catalog ⋈ σ_{color≠'red'}(parts) )
-->

14. `sid` de proveedores que suministran todas las partes.

```sql
SELECT c.sid
FROM catalog c
GROUP BY c.sid
HAVING COUNT(DISTINCT c.pid) = (SELECT COUNT(*) FROM parts);
```

<!--
RA: π_{sid,pid}(catalog) ÷ π_{pid}(parts)
-->

15. Nombres y salarios de jefes que tienen algún empleado con salario > 100.

```sql
SELECT DISTINCT b.name, b.salary
FROM employees b
JOIN supervises sv ON sv.boss = b.number
JOIN employees  e  ON e.number = sv.employee
WHERE e.salary > 100;
```

<!--
RA: π_{b.name, b.salary}(
       σ_{e.salary>100}(
          ρ_{b}(employees) ⋈_{b.number=sv.boss} ρ_{sv}(supervises)
                            ⋈_{sv.employee=e.number} ρ_{e}(employees)
       )
    )
-->

16. Pares (jefe, empleado) donde el empleado gana más que su jefe.

```sql
SELECT b.name AS jefe, e.name AS empleado
FROM supervises sv
JOIN employees b ON b.number = sv.boss
JOIN employees e ON e.number = sv.employee
WHERE e.salary > b.salary;
```

<!--
RA: π_{b.name, e.name}(
       σ_{e.salary>b.salary}(
          supervises ⋈_{boss=b.number} ρ_{b}(employees)
                     ⋈_{employee=e.number} ρ_{e}(employees)
       )
    )
-->

17. Nombres de empleados que no tienen jefe.

```sql
SELECT name FROM employees
WHERE number NOT IN (SELECT employee FROM supervises);
```

<!--
RA: π_{name}( employees ⋈_{number=x} ρ_{x}( π_{number}(employees) − π_{employee}(supervises) ) )
-->

18. Hora de cita y nombre del cliente para las citas de *Giuliano* el 2026-02-14.

```sql
SELECT c.name, a.atime
FROM appointments a
JOIN staff   s ON s.sid = a.sid
JOIN clients c ON c.cid = a.cid
WHERE a.adate = DATE '2026-02-14' AND s.name = 'Giuliano';
```

<!--
RA: π_{c.name, a.atime}(
       σ_{a.adate='2026-02-14' ∧ s.name='Giuliano'}(
          ρ_{a}(appointments) ⋈_{a.sid=s.sid} ρ_{s}(staff)
                              ⋈_{a.cid=c.cid} ρ_{c}(clients)
       )
    )
-->

19. Servicios que han sido solicitados al menos una vez.

```sql
SELECT DISTINCT service FROM appointments;
```

<!--
RA: π_{service}(appointments)
-->

20. Clientes (nombre y teléfono) que nunca tomaron el servicio *manicure*.

```sql
SELECT c.name, c.phone
FROM clients c
WHERE c.cid NOT IN (SELECT cid FROM appointments WHERE service = 'manicure');
```

<!--
RA: π_{name, phone}(clients) − π_{name, phone}( clients ⋈ σ_{service='manicure'}(appointments) )
-->

---

### **2. Álgebra Relacional → SQL**

Para cada expresión, escribe la consulta SQL equivalente.

1. $\pi_{sid}\bigl(\sigma_{rating > 7}(sailors)\bigr)$

<!--
SQL: SELECT sid FROM sailors WHERE rating > 7;
-->

2. $\pi_{sname}\bigl(\sigma_{age \geq 18 \wedge age \leq 25}(sailors)\bigr)$

<!--
SQL: SELECT sname FROM sailors WHERE age BETWEEN 18 AND 25;
-->

3. $\pi_{sname}\bigl(sailors \bowtie reserves \bowtie \sigma_{color='red'}(boats)\bigr)$

<!--
SQL:
  SELECT DISTINCT s.sname
  FROM sailors s
  JOIN reserves r ON r.sid = s.sid
  JOIN boats    b ON b.bid = r.bid
  WHERE b.color = 'red';
-->

4. $\pi_{s1.sid}\bigl(\rho_{s1}(sailors) \bowtie_{s1.rating > s2.rating} \rho_{s2}(\sigma_{sname='Bob'}(sailors))\bigr)$

<!--
SQL:
  SELECT DISTINCT s1.sid
  FROM sailors s1 JOIN sailors s2 ON s1.rating > s2.rating
  WHERE s2.sname = 'Bob';
-->

5. $\pi_{sid}(sailors)\;-\;\pi_{s1.sid}\bigl(\rho_{s1}(sailors) \bowtie_{s1.rating < s2.rating} \rho_{s2}(sailors)\bigr)$

<!--
SQL:
  SELECT sid FROM sailors
  WHERE rating = (SELECT MAX(rating) FROM sailors);
-->

6. $\pi_{pname}\bigl(\sigma_{color='red'}(parts)\bigr)$

<!--
SQL: SELECT pname FROM parts WHERE color = 'red';
-->

7. $\pi_{cost}\bigl(\sigma_{color='red' \vee color='green'}(parts) \bowtie catalog\bigr)$

<!--
SQL:
  SELECT DISTINCT c.cost
  FROM catalog c JOIN parts p ON p.pid = c.pid
  WHERE p.color IN ('red','green');
-->

8. $\pi_{sid}\bigl(\sigma_{color='red' \vee color='green'}(parts) \bowtie catalog\bigr)$

<!--
SQL:
  SELECT DISTINCT c.sid
  FROM catalog c JOIN parts p ON p.pid = c.pid
  WHERE p.color IN ('red','green');
-->

9. $\pi_{sname}\Bigl(\pi_{sid}\bigl(\sigma_{color='red' \vee color='green'}(parts) \bowtie catalog\bigr) \bowtie suppliers\Bigr)$

<!--
SQL:
  SELECT DISTINCT s.sname
  FROM suppliers s
  JOIN catalog c ON c.sid = s.sid
  JOIN parts   p ON p.pid = c.pid
  WHERE p.color IN ('red','green');
-->

10. $\pi_{sname}\bigl(\sigma_{color='red'}(parts) \bowtie \sigma_{cost < 100}(catalog) \bowtie suppliers\bigr)$

<!--
SQL:
  SELECT DISTINCT s.sname
  FROM suppliers s
  JOIN catalog c ON c.sid = s.sid
  JOIN parts   p ON p.pid = c.pid
  WHERE p.color = 'red' AND c.cost < 100;
-->

11. $\pi_{sname}(\sigma_{color='red'}(parts) \bowtie \sigma_{cost<100}(catalog) \bowtie suppliers)\;\cap\;\pi_{sname}(\sigma_{color='green'}(parts) \bowtie \sigma_{cost<100}(catalog) \bowtie suppliers)$

<!--
SQL:
  SELECT s.sname
  FROM suppliers s JOIN catalog c ON c.sid=s.sid JOIN parts p ON p.pid=c.pid
  WHERE p.color='red' AND c.cost<100
  INTERSECT
  SELECT s.sname
  FROM suppliers s JOIN catalog c ON c.sid=s.sid JOIN parts p ON p.pid=c.pid
  WHERE p.color='green' AND c.cost<100;
-->

12. $\pi_{sid}(\sigma_{color='red'}(parts) \bowtie \sigma_{cost<100}(catalog))\;\cap\;\pi_{sid}(\sigma_{color='green'}(parts) \bowtie \sigma_{cost<100}(catalog))$

<!--
SQL:
  SELECT c.sid FROM catalog c JOIN parts p ON p.pid=c.pid
  WHERE p.color='red' AND c.cost<100
  INTERSECT
  SELECT c.sid FROM catalog c JOIN parts p ON p.pid=c.pid
  WHERE p.color='green' AND c.cost<100;
-->

13. $\pi_{sid}(suppliers)\;-\;\pi_{sid}(catalog)$

<!--
SQL: SELECT sid FROM suppliers EXCEPT SELECT sid FROM catalog;
-->

14. $\bigl(\pi_{sid}(catalog) \times \pi_{pid}(parts)\bigr)\;-\;\pi_{sid,pid}(catalog)$

<!--
SQL:
  SELECT c.sid, p.pid
  FROM (SELECT DISTINCT sid FROM catalog) c CROSS JOIN parts p
  EXCEPT
  SELECT sid, pid FROM catalog;
-->

15. $\pi_{name}\bigl(\sigma_{salary > 1000 \wedge age < 30}(employees)\bigr)$

<!--
SQL: SELECT name FROM employees WHERE salary > 1000 AND age < 30;
-->

16. $\pi_{b.name}\bigl(\rho_{b}(employees) \bowtie_{b.number=sv.boss} \rho_{sv}(supervises)\bigr)$

<!--
SQL:
  SELECT DISTINCT b.name
  FROM employees b JOIN supervises sv ON sv.boss = b.number;
-->

17. $\pi_{number}(employees)\;-\;\pi_{boss}(supervises)$

<!--
SQL:
  SELECT number FROM employees
  EXCEPT
  SELECT boss FROM supervises;
-->

18. $\pi_{name, phone}\bigl(clients \bowtie \sigma_{service='haircut'}(appointments)\bigr)$

<!--
SQL:
  SELECT DISTINCT c.name, c.phone
  FROM clients c JOIN appointments a ON a.cid = c.cid
  WHERE a.service = 'haircut';
-->

19. $\pi_{cid}(clients)\;-\;\pi_{cid}(appointments)$

<!--
SQL:
  SELECT cid FROM clients EXCEPT SELECT cid FROM appointments;
-->

20. $\pi_{c.name, s.name}\bigl(\rho_{c}(clients) \bowtie_{c.cid=a.cid} \rho_{a}(appointments) \bowtie_{a.sid=s.sid} \rho_{s}(staff)\bigr)$

<!--
SQL:
  SELECT DISTINCT c.name AS cliente, s.name AS atendido_por
  FROM clients c
  JOIN appointments a ON a.cid = c.cid
  JOIN staff       s ON s.sid = a.sid;
-->

---

### **Fuentes**

- [UBC CPSC 304 — *Exercises on Relational Algebra and Datalog (Part I)*](https://www.cs.ubc.ca/~laks/cpsc304/RA-Datalog-Tutorial%20-%20Sol.pdf)
  (esquema Sailors–Boats–Reserves).
- [University of Toronto CSC343 — *Relational Algebra Exercises for Tutorial*](https://www.eecs.yorku.ca/~papaggel/courses/eecs3421/docs/tutorials/tut1-ra.pdf)
  (esquemas Suppliers–Parts–Catalog, Employees–Supervises, Clients–Staff–Appointments).
- [Free University of Bozen-Bolzano — *Introduction to Databases: Relational Algebra Sample Solutions*](https://www.inf.unibz.it/~nutt/Teaching/IDBs0910/IDBExercises/4-sol-relAlg.pdf)
  (esquema Suppliers–Parts–Catalog).
