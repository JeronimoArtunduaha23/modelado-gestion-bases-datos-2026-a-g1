# Cardinalidad

## ¿Cuántas instancias de una entidad se relacionan con cuántas de otra?

La **cardinalidad** define la cantidad **máxima** de relación entre entidades.

---

## Tipos de Cardinalidad

### 1:1 (Uno a Uno)

Una entidad se relaciona con una sola instancia de la otra.

**Ejemplo:**
Persona — tiene — Cédula

---

### 1:N (Uno a Muchos)

Una instancia se relaciona con muchas, pero del otro lado solo con una.

**Ejemplo del caso:**

PROGRAMA — ESTUDIANTE

- Un programa tiene muchos estudiantes.
- Un estudiante pertenece a un solo programa.

→ **Cardinalidad: 1:N**

---

### N:M (Muchos a Muchos)

Ambas entidades pueden relacionarse con muchas instancias de la otra.

**Ejemplo clave:**

ESTUDIANTE — cursa — ASIGNATURA

- Un estudiante puede cursar muchas asignaturas.
- Una asignatura puede tener muchos estudiantes.

→ **Cardinalidad: N:M**

> En base de datos se crea una tabla intermedia para resolver esta relación.

---

# Participación

## ¿Es obligatorio u opcional participar en la relación?

La **participación** define la cantidad **mínima** de participación en una relación.

---

## Tipos de Participación

### Participación Total (Obligatoria)

La entidad debe participar en la relación.

**Ejemplo:**

Un estudiante debe pertenecer a un programa.  
→ Participación total del lado ESTUDIANTE.

---

### Participación Parcial (Opcional)

La entidad puede existir sin participar en la relación.

**Ejemplo:**

Un docente puede existir sin estar dictando asignaturas en el semestre actual.  
→ Participación parcial del lado DOCENTE.

---

# Diferencia Clave

- **Cardinalidad** = máximo permitido.
- **Participación** = mínimo obligatorio.

---
# Importante

> La cardinalidad define cuántas instancias pueden relacionarse, mientras que la participación define si esa relación es obligatoria u opcional según las reglas del negocio.
