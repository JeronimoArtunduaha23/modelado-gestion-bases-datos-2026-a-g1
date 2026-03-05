# Sistema de Formación Técnica – Modelado de Base de Datos

## 1. Portada

**Asignatura:** Modelado y Gestión de Bases de Datos  
**Estudiante:** Jeronimo  
**Código:** TU_CODIGO  
**Grupo:** TU_GRUPO  
**Docente:** NOMBRE_DOCENTE  
**Fecha:** FECHA_DE_ENTREGA  

---

# 2. Análisis del Dominio

El sistema de formación técnica tiene como objetivo gestionar la información académica de una plataforma educativa que ofrece cursos en modalidad presencial y virtual. Cada curso pertenece a un área de conocimiento específica y puede tener cursos prerrequisito que los estudiantes deben cumplir antes de inscribirse.

Los cursos se ofertan mediante cohortes en distintos periodos académicos. Cada cohorte posee un código único, modalidad (virtual o presencial) y un cupo máximo de estudiantes.

Los instructores son los encargados de dictar las cohortes. Un instructor puede dictar múltiples cohortes y además algunos instructores pueden estar matriculados como estudiantes en otras cohortes. Esto implica que una persona puede desempeñar más de un rol dentro del sistema.

Los estudiantes pueden inscribirse en varias cohortes en un periodo académico, pero no pueden inscribirse dos veces en la misma cohorte. Esta relación se gestiona mediante una entidad llamada matrícula.

Al finalizar determinados cursos se otorgan certificaciones oficiales. Estas certificaciones pueden estar asociadas a uno o varios cursos dependiendo de los requisitos establecidos.

Las entidades principales identificadas son:

- Persona
- Estudiante
- Instructor
- Área de Conocimiento
- Curso
- Cohorte
- Certificación

Además se identifican entidades asociativas necesarias para resolver relaciones de muchos a muchos:

- Matrícula
- Instructor_Cohorte
- Certificación_Curso
- Curso_Prerequisito

Este análisis permite construir un modelo conceptual sólido para el diseño de la base de datos.

---

# 3. Diagrama Entidad Relación

El modelo incluye las siguientes entidades principales:

- Persona
- Estudiante
- Instructor
- Área_Conocimiento
- Curso
- Cohorte
- Certificación

Relaciones principales:

- Área → Curso (1:N)
- Curso → Cohorte (1:N)
- Estudiante → Cohorte (N:M)
- Instructor → Cohorte (N:M)
- Curso → Certificación (N:M)
- Curso → Curso (Prerrequisito)

Generalización:

PERSONA  
△  
ESTUDIANTE / INSTRUCTOR

Tipo de especialización: **Overlap (superpuesta)**.

---

# 4. Justificación Técnica

La entidad Persona se utiliza como supertipo para representar a todas las personas dentro del sistema. A partir de esta entidad se derivan las entidades Estudiante e Instructor mediante una generalización.

La especialización se definió como superpuesta (overlap), ya que una misma persona puede desempeñar ambos roles dentro del sistema, es decir, puede ser instructor y estudiante al mismo tiempo.

Las relaciones de muchos a muchos se resolvieron mediante entidades asociativas. Por ejemplo, la relación entre estudiantes y cohortes se resolvió mediante la entidad Matrícula, que permite registrar la inscripción de un estudiante en una cohorte específica.

La relación entre instructores y cohortes se resolvió mediante la entidad Instructor_Cohorte, permitiendo que un instructor pueda dictar múltiples cohortes y que una cohorte pueda tener varios instructores.

La relación entre cursos y certificaciones se modeló como una relación de muchos a muchos, ya que una certificación puede requerir la aprobación de varios cursos.

Los prerrequisitos entre cursos se modelaron mediante una relación recursiva denominada Curso_Prerequisito.

Estas decisiones permiten mantener el modelo flexible, normalizado y coherente con las reglas del dominio.

---

# 5. Modelo Relacional

## PERSONA

| Atributo |
|---|
| **id_persona (PK)** |
| nombre |
| apellido |
| correo |
| telefono |

---

## ESTUDIANTE

| Atributo |
|---|
| **id_persona (PK, FK)** |
| fecha_registro |

FK → PERSONA(id_persona)

---

## INSTRUCTOR

| Atributo |
|---|
| **id_persona (PK, FK)** |
| especialidad |
| experiencia |

FK → PERSONA(id_persona)

---

## AREA_CONOCIMIENTO

| Atributo |
|---|
| **id_area (PK)** |
| nombre |
| descripcion |

---

## CURSO

| Atributo |
|---|
| **id_curso (PK)** |
| nombre |
| descripcion |
| id_area (FK) |

FK → AREA_CONOCIMIENTO(id_area)

---

## COHORTE

| Atributo |
|---|
| **id_cohorte (PK)** |
| periodo_academico |
| modalidad |
| cupo_maximo |
| id_curso (FK) |

FK → CURSO(id_curso)

---

## MATRICULA

| Atributo |
|---|
| **id_matricula (PK)** |
| id_persona (FK) |
| id_cohorte (FK) |
| fecha_matricula |

FK → ESTUDIANTE(id_persona)  
FK → COHORTE(id_cohorte)

---

## INSTRUCTOR_COHORTE

| Atributo |
|---|
| **id_persona (PK, FK)** |
| **id_cohorte (PK, FK)** |

FK → INSTRUCTOR(id_persona)  
FK → COHORTE(id_cohorte)

---

## CERTIFICACION

| Atributo |
|---|
| **id_certificacion (PK)** |
| nombre |
| descripcion |

---

## CERTIFICACION_CURSO

| Atributo |
|---|
| **id_certificacion (PK, FK)** |
| **id_curso (PK, FK)** |

FK → CERTIFICACION(id_certificacion)  
FK → CURSO(id_curso)

---

## CURSO_PREREQUISITO

| Atributo |
|---|
| **id_curso (PK, FK)** |
| **id_curso_prerequisito (PK, FK)** |

FK → CURSO(id_curso)  
FK → CURSO(id_curso_prerequisito)

---