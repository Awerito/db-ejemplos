## **Actividad en Clase: Álgebra y Cálculo Relacional sobre Sailors-Boats-Reserves**

⏳ **Duración total: 90 minutos**  
🎯 **Objetivo:** Practicar los operadores del **álgebra relacional**
($\sigma, \pi, \cup, \cap, -, \times, \rho, \bowtie, \div$) y el **cálculo
relacional** (de tuplas y de dominios) traduciendo consultas en lenguaje
natural a las tres notaciones, y verificándolas en SQL.

---

1. **Trabajo individual o en parejas.**
2. **Pre-requisito:** haber visto la clase
   [09 — Álgebra y Cálculo Relacional](https://github.com/Awerito/base-de-datos-apuntes/tree/master/09-algebra-calculo-relacional).
3. **Esquema de trabajo** (clásico Ramakrishnan & Gehrke):

```
Sailors(sid, sname, rating, age)
Boats(bid, bname, color)
Reserves(sid, bid, day)
```

con `Reserves.sid → Sailors.sid` y `Reserves.bid → Boats.bid`.

4. **Configuración:** crear las 3 tablas con sus PK/FK en PostgreSQL e
   insertar datos de prueba (puedes usar
   [este dataset de ejemplo](https://github.com/yannisbros/sailors-boats-reserves)
   o generar tuplas mínimas que cubran los casos de cada ejercicio:
   marineros con rating alto/bajo, botes rojos/no rojos, un bote
   "BigBoat", marineros llamados Albert y Bob, reservas múltiples).

---

### **📋 Instrucciones**

Para cada ejercicio entrega:

1. **Álgebra relacional** (notación $\sigma, \pi, \bowtie, \div, \ldots$).
2. **Cálculo relacional** de tuplas o de dominios (a tu elección).
3. **Consulta SQL ejecutable** que produce el mismo resultado.

---

### **📐 Selección, proyección y unión**

✅ Operadores $\sigma$, $\pi$, $\cup$.

1. Encontrar los **colores** de los botes reservados por el marinero
   llamado *Albert*.

<!--
RA:  π_color( σ_sname='Albert'(Sailors) ⋈ Reserves ⋈ Boats )
TRC: { t.color | t ∈ Boats ∧ ∃r ∈ Reserves, ∃s ∈ Sailors
                 ( r.bid = t.bid ∧ r.sid = s.sid ∧ s.sname = 'Albert' ) }
SQL:
  SELECT DISTINCT b.color
  FROM Sailors s
  JOIN Reserves r ON r.sid = s.sid
  JOIN Boats    b ON b.bid = r.bid
  WHERE s.sname = 'Albert';
-->

2. Encontrar los `sid` de marineros que tengan rating ≥ 8 **o** que
   hayan reservado el bote 103.

<!--
RA:  π_sid( σ_rating≥8(Sailors) ) ∪ π_sid( σ_bid=103(Reserves) )
SQL:
  SELECT sid FROM Sailors  WHERE rating >= 8
  UNION
  SELECT sid FROM Reserves WHERE bid = 103;
-->


---

### **➖ Diferencia y negación**

✅ Operador $-$ y patrón equivalente con `LEFT JOIN ... IS NULL`.

3. Encontrar los **nombres** de marineros que **no** han reservado un
   bote rojo.

<!--
RA:  π_sname( ( π_sid(Sailors) − π_sid( σ_color='red'(Boats) ⋈ Reserves ) ) ⋈ Sailors )
SQL (EXCEPT):
  SELECT s.sname
  FROM Sailors s
  WHERE s.sid IN (
    SELECT sid FROM Sailors
    EXCEPT
    SELECT r.sid FROM Reserves r JOIN Boats b ON b.bid = r.bid WHERE b.color = 'red'
  );
SQL (LEFT JOIN ... IS NULL):
  SELECT DISTINCT s.sname
  FROM Sailors s
  LEFT JOIN Reserves r ON r.sid = s.sid
  LEFT JOIN Boats    b ON b.bid = r.bid AND b.color = 'red'
  WHERE b.bid IS NULL;
-->

4. Encontrar los `sid` de marineros con edad mayor a 20 que **no** han
   reservado un bote rojo.

<!--
RA:  π_sid( σ_age>20(Sailors) ) − π_sid( σ_color='red'(Boats) ⋈ Reserves )
SQL:
  SELECT sid FROM Sailors WHERE age > 20
  EXCEPT
  SELECT r.sid FROM Reserves r
  JOIN Boats b ON b.bid = r.bid
  WHERE b.color = 'red';
-->

> Resuelve el ejercicio 3 también con `EXCEPT` y con
> `LEFT JOIN ... WHERE ... IS NULL`. Compara ambos planes con `EXPLAIN`.

---

### **🔁 Autoreferencia con renombrado ($\rho$)**

✅ Operador $\rho$ y comparación de una relación consigo misma.

5. Encontrar los nombres de marineros que han reservado **al menos dos**
   botes distintos.

<!--
RA:  π_sname( σ_r1.sid=r2.sid ∧ r1.bid≠r2.bid ( Reserves × ρ_r2(Reserves) ) ⋈ Sailors )
SQL:
  SELECT DISTINCT s.sname
  FROM Sailors s
  JOIN Reserves r1 ON r1.sid = s.sid
  JOIN Reserves r2 ON r2.sid = s.sid AND r2.bid <> r1.bid;
-->

6. Encontrar los `sid` de marineros cuyo rating es mejor que el de
   **algún** marinero llamado *Bob*.

<!--
RA:  π_s.sid( σ_s.rating>b.rating ( Sailors × ρ_b(σ_sname='Bob'(Sailors)) ) )
SQL:
  SELECT s.sid
  FROM Sailors s
  WHERE s.rating > (SELECT MIN(rating) FROM Sailors WHERE sname = 'Bob');
-->

7. Encontrar los `sid` de marineros cuyo rating es mejor que el de
   **todos** los marineros llamados *Bob*.

<!--
RA:  π_sid(Sailors) − π_s.sid( σ_s.rating≤b.rating ( Sailors × ρ_b(σ_sname='Bob'(Sailors)) ) )
SQL:
  SELECT s.sid
  FROM Sailors s
  WHERE s.rating > ALL (SELECT rating FROM Sailors WHERE sname = 'Bob');
-->


---

### **➗ División ($\div$)**

✅ Patrón "para todo / NOT EXISTS anidado".

8. Encontrar los nombres de marineros que han reservado **todos** los
   botes.

<!--
RA:  π_sname( ( π_sid,bid(Reserves) ÷ π_bid(Boats) ) ⋈ Sailors )
SQL (NOT EXISTS):
  SELECT s.sname
  FROM Sailors s
  WHERE NOT EXISTS (
    SELECT 1 FROM Boats b
    WHERE NOT EXISTS (
      SELECT 1 FROM Reserves r
      WHERE r.sid = s.sid AND r.bid = b.bid
    )
  );
SQL (HAVING COUNT):
  SELECT s.sname
  FROM Sailors s
  JOIN Reserves r ON r.sid = s.sid
  GROUP BY s.sid, s.sname
  HAVING COUNT(DISTINCT r.bid) = (SELECT COUNT(*) FROM Boats);
-->

9. Encontrar los nombres de marineros que han reservado todos los botes
   llamados *BigBoat*.

<!--
RA:  π_sname( ( π_sid,bid(Reserves) ÷ π_bid( σ_bname='BigBoat'(Boats) ) ) ⋈ Sailors )
SQL:
  SELECT s.sname
  FROM Sailors s
  WHERE NOT EXISTS (
    SELECT 1 FROM Boats b
    WHERE b.bname = 'BigBoat'
      AND NOT EXISTS (
        SELECT 1 FROM Reserves r
        WHERE r.sid = s.sid AND r.bid = b.bid
      )
  );
-->

10. Encontrar los nombres de marineros que han reservado **todos** los
    botes que han sido reservados por marineros con menor rating que
    ellos.

<!--
RA (siguiendo el tutorial UBC, con abreviaciones):
  sALS  ← π_s.sid,s2.sid( σ_s.rating>s2.rating ( Sailors × ρ_s2(Sailors) ) )
  sHR   ← π_s.sid,bid( sALS ⋈_{s2.sid=r.sid} Reserves )    -- "should have reserved"
  wOD   ← sHR − π_sid,bid(Reserves)                         -- "witness of disqualification"
  ans   ← π_s2.sname( ( π_sid(Sailors) − π_sid(wOD) ) ⋈ ρ_s2(Sailors) )
SQL:
  SELECT s.sname
  FROM Sailors s
  WHERE NOT EXISTS (
    SELECT 1
    FROM Sailors s2
    JOIN Reserves r2 ON r2.sid = s2.sid
    WHERE s2.rating < s.rating
      AND NOT EXISTS (
        SELECT 1 FROM Reserves r
        WHERE r.sid = s.sid AND r.bid = r2.bid
      )
  );
-->

> Resuelve al menos uno de estos en SQL de **dos** formas: con doble
> `NOT EXISTS` y con `GROUP BY ... HAVING COUNT(DISTINCT ...) = (SELECT
> COUNT(*) FROM ...)`.

---

### **🏆 Máximo / mínimo sin agregados**

✅ Truco clásico: "no existe nadie mejor".

11. Encontrar los `sid` de marineros con el **rating más alto**, sin
    usar `MAX` ni `ORDER BY ... LIMIT`.

<!--
RA:  π_sid(Sailors) − π_s.sid( σ_s.rating<s2.rating ( Sailors × ρ_s2(Sailors) ) )
SQL:
  SELECT s.sid
  FROM Sailors s
  WHERE NOT EXISTS (
    SELECT 1 FROM Sailors s2 WHERE s2.rating > s.rating
  );
-->

12. Encontrar el nombre y la edad del marinero **más viejo**, sin usar
    `MAX` ni `ORDER BY ... LIMIT`.

<!--
RA:  π_sname,age( ( π_sid(Sailors) − π_s.sid( σ_s.age<s2.age ( Sailors × ρ_s2(Sailors) ) ) ) ⋈ Sailors )
SQL:
  SELECT s.sname, s.age
  FROM Sailors s
  WHERE NOT EXISTS (
    SELECT 1 FROM Sailors s2 WHERE s2.age > s.age
  );
-->


---

### **📝 Entregable**

Un archivo `algebra.md` (o `algebra.sql` con bloques de comentario) que
contenga, para cada uno de los 12 ejercicios, las tres formas pedidas.
Las consultas SQL deben ejecutarse sin error sobre tu instancia con los
datos de prueba.

---

💬 **Reflexión final**

- ¿Por qué la división relacional ($\div$) es difícil de expresar en SQL
  sin `NOT EXISTS` anidados?
- ¿Qué diferencias prácticas hay entre `EXCEPT` y `LEFT JOIN ... IS
  NULL` cuando el resultado puede contener duplicados?
- ¿Cuándo te resultó más cómodo razonar en cálculo de tuplas (declarar
  *qué* quieres) y cuándo en álgebra (decir *cómo* obtenerlo)?
- ¿Qué consultas requirieron usar $\rho$ explícitamente y por qué?

---

### **📚 Fuentes**

- Ramakrishnan, R. & Gehrke, J. *Database Management Systems*, 3rd ed.,
  McGraw-Hill (capítulo 4: "Relational Algebra and Calculus", esquema
  Sailors-Boats-Reserves).
- UBC CPSC 304 — *Exercises on Relational Algebra and Datalog, Part I*
  (Laks V.S. Lakshmanan):
  <https://www.cs.ubc.ca/~laks/cpsc304/RA-Datalog-Tutorial%20-%20Sol.pdf>
