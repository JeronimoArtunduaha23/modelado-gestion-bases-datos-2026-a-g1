# Taller – Modelo Entidad Relación  

## Sistema de Gestión Académica

# Introducción

En la actualidad, los sistemas de información desempeñan un papel fundamental en la gestión eficiente de las instituciones educativas. La correcta administración de estudiantes, cursos, docentes y matrículas permite optimizar los procesos académicos, mejorar la toma de decisiones y garantizar un adecuado control de la información.

El presente taller tiene como objetivo aplicar los conceptos fundamentales del Modelo Entidad-Relación (ER) y su posterior transformación al modelo relacional, a partir del diseño de un sistema para la gestión académica. A través del análisis de datos, la definición de entidades, atributos, relaciones y cardinalidades, se construye una estructura lógica que representa de manera clara y organizada la realidad del entorno educativo.

Finalmente, se incorpora el concepto de generalización mediante la entidad Persona, permitiendo evidenciar la aplicación de principios avanzados de modelado de datos, asegurando coherencia, integridad y escalabilidad en el diseño del sistema.

---
# 1. Datos vs Información


## Datos que manejaría el sistema

1. id_persona  
2. nombre  
3. documento  
4. fecha_nacimiento  
5. correo  
6. id_curso  
7. nombre_curso  
8. creditos  
9. descripcion  
10. id_matricula  
11. fecha_matricula  
12. estado_matricula  
13. especialidad  

---

## Transformación de Datos en Información

| Datos utilizados | Información generada |
|------------------|----------------------|
| fecha_matricula + estado_matricula | Cantidad de estudiantes activos por semestre |
| id_curso + creditos | Carga académica total por estudiante |
| id_docente + id_curso | Cursos asignados por docente |
| fecha_nacimiento | Promedio de edad estudiantil |
| estado_matricula | Índice de deserción |

### Justificación
Los datos son registros individuales almacenados en el sistema. Al organizarlos y analizarlos se transforman en información estratégica que apoya la toma de decisiones académicas y administrativas.

---

# 2. Modelo Entidad – Relación (ER)

## Entidades y Atributos

### Persona
- **id_persona (PK)**
- nombre
- documento
- fecha_nacimiento
- correo

### Estudiante
- **id_persona (PK, FK)**

### Docente
- **id_persona (PK, FK)**
- especialidad

### Curso
- **id_curso (PK)**
- nombre_curso
- creditos
- descripcion
- id_docente (FK)

### Matrícula
- **id_matricula (PK)**
- fecha_matricula
- estado
- id_persona (FK)
- id_curso (FK)

---



### Justificación
La entidad Matrícula permite almacenar información adicional sobre la inscripción y resolver la relación muchos a muchos entre estudiantes y cursos.

---

# 3. Relación Docente – Curso

## Cardinalidad
Docente (1) —— (N) Curso

### Justificación
Un docente puede dictar varios cursos en un periodo académico, pero cada curso tiene un único docente responsable.

---

# 4. Generalización: Persona → Estudiante / Docente

## Tipo de Especialización

- **Total**
- **Disjoint (Excluyente)**

### Justificación
Es total porque toda persona registrada debe ser estudiante o docente.  
Es disjoint porque en este modelo simplificado una persona no puede desempeñar ambos roles simultáneamente.

---

# 5. Conversión al Modelo Relacional

## Tabla: PERSONA
| Campo | Tipo |
|-------|------|
| id_persona (PK) | INT |
| nombre | VARCHAR |
| documento | VARCHAR |
| fecha_nacimiento | DATE |
| correo | VARCHAR |

---

## Tabla: ESTUDIANTE
| Campo | Tipo |
|-------|------|
| id_persona (PK, FK) | INT |

---

## Tabla: DOCENTE
| Campo | Tipo |
|-------|------|
| id_persona (PK, FK) | INT |
| especialidad | VARCHAR |

---

## Tabla: CURSO
| Campo | Tipo |
|-------|------|
| id_curso (PK) | INT |
| nombre_curso | VARCHAR |
| creditos | INT |
| descripcion | VARCHAR |
| id_persona_docente (FK) | INT |

---

## Tabla: MATRICULA
| Campo | Tipo |
|-------|------|
| id_matricula (PK) | INT |
| fecha_matricula | DATE |
| estado | VARCHAR |
| id_persona_estudiante (FK) | INT |
| id_curso (FK) | INT |

---

## Justificación Final
La generalización se implementa mediante herencia por clave primaria compartida.  
Las relaciones 1:N se representan con claves foráneas.  
La relación muchos a muchos se transforma en la tabla intermedia MATRÍCULA.

---