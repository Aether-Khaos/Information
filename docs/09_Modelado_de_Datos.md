# 09 — Modelo de Datos

## 9.1 Motor de base de datos

PostgreSQL 15+. Se eligió por la naturaleza relacional del dominio académico:

- Estudiantes ↔ tutores (1:1).
- Docentes ↔ materias ↔ cursos (N:M vía `teacher_assignments`).
- Notas ↔ estudiantes ↔ materias ↔ trimestres (N:M con atributos).
- Asistencia ↔ estudiantes ↔ materias ↔ fechas.

## 9.2 Tablas

### `users`

| Columna | Tipo | Restricciones |
|---|---|---|
| `id` | SERIAL | PK |
| `email` | VARCHAR(100) | UNIQUE NOT NULL |
| `role` | VARCHAR(20) | NOT NULL, CHECK IN ('admin','teacher','staff','student','parent') |
| `teacher_id` | INTEGER | FK → teachers(id), nullable |
| `staff_id` | INTEGER | FK, nullable |
| `student_id` | INTEGER | FK → students(id), nullable |
| `parent_of_student_id` | INTEGER | FK → students(id), nullable |
| `is_active` | BOOLEAN | DEFAULT true |
| `created_at` | TIMESTAMPTZ | DEFAULT NOW() |
| `updated_at` | TIMESTAMPTZ | DEFAULT NOW() |

### `students`

| Columna | Tipo | Restricciones |
|---|---|---|
| `id` | SERIAL | PK |
| `first_names` | VARCHAR(100) | NOT NULL |
| `paternal_surname` | VARCHAR(100) | NOT NULL |
| `maternal_surname` | VARCHAR(100) | NOT NULL |
| `tutor_email` | VARCHAR(100) | NOT NULL |
| `student_email` | VARCHAR(100) | NULL |
| `ci` | VARCHAR(8) | UNIQUE NOT NULL |
| `rude` | VARCHAR(20) | UNIQUE NOT NULL |
| `birth_date` | DATE | NOT NULL |
| `grade_level` | VARCHAR(50) | NOT NULL |
| `paralelo` | VARCHAR(10) | NOT NULL |
| `tutor_first_names` | VARCHAR(100) | NOT NULL |
| `tutor_paternal_surname` | VARCHAR(100) | NOT NULL |
| `tutor_maternal_surname` | VARCHAR(100) | NOT NULL |
| `tutor_phone` | VARCHAR(8) | NOT NULL |
| `created_at` | TIMESTAMPTZ | DEFAULT NOW() |

### `teachers`

| Columna | Tipo | Restricciones |
|---|---|---|
| `id` | SERIAL | PK |
| `first_names` | VARCHAR(100) | NOT NULL |
| `paternal_surname` | VARCHAR(100) | NOT NULL |
| `maternal_surname` | VARCHAR(100) | NOT NULL |
| `ci` | VARCHAR(8) | UNIQUE NOT NULL |
| `email` | VARCHAR(100) | UNIQUE NOT NULL |
| `phone` | VARCHAR(8) | NOT NULL |
| `birth_date` | DATE | NOT NULL |
| `specialty` | VARCHAR(100) | NOT NULL |
| `hire_date` | DATE | NOT NULL |
| `is_active` | BOOLEAN | DEFAULT true |
| `created_at` | TIMESTAMPTZ | DEFAULT NOW() |

### `subjects`

| Columna | Tipo | Restricciones |
|---|---|---|
| `id` | SERIAL | PK |
| `name` | VARCHAR(100) | NOT NULL |
| `level` | VARCHAR(20) | CHECK IN ('inicial','primaria','secundaria') |
| `is_active` | BOOLEAN | DEFAULT true |
| `created_at` | TIMESTAMPTZ | DEFAULT NOW() |

### `teacher_assignments`

| Columna | Tipo | Restricciones |
|---|---|---|
| `id` | SERIAL | PK |
| `teacher_id` | INTEGER | FK → teachers(id) |
| `subject_id` | INTEGER | FK → subjects(id) |
| `grade_level` | VARCHAR(50) | NOT NULL |
| `paralelo` | VARCHAR(10) | NOT NULL |
| `created_at` | TIMESTAMPTZ | DEFAULT NOW() |
| — | — | UNIQUE(teacher_id, subject_id, grade_level, paralelo) |

### `grades`

| Columna | Tipo | Restricciones |
|---|---|---|
| `id` | SERIAL | PK |
| `student_id` | INTEGER | FK → students(id) |
| `teacher_id` | INTEGER | FK → teachers(id) |
| `subject_id` | INTEGER | FK → subjects(id) |
| `grade_level` | VARCHAR(50) | NOT NULL |
| `paralelo` | VARCHAR(10) | NOT NULL |
| `period` | VARCHAR(20) | CHECK IN ('1er Trimestre','2do Trimestre','3er Trimestre') |
| `ser` | NUMERIC(4,2) | NOT NULL |
| `saber` | NUMERIC(4,2) | NOT NULL |
| `hacer` | NUMERIC(4,2) | NOT NULL |
| `decidir` | NUMERIC(4,2) | NOT NULL |
| `autoevaluacion` | NUMERIC(4,2) | NOT NULL |
| `total` | NUMERIC(4,2) | NOT NULL |
| `created_at` | TIMESTAMPTZ | DEFAULT NOW() |
| `updated_at` | TIMESTAMPTZ | DEFAULT NOW() |
| — | — | UNIQUE(student_id, subject_id, period) |

### `attendance`

| Columna | Tipo | Restricciones |
|---|---|---|
| `id` | SERIAL | PK |
| `student_id` | INTEGER | FK → students(id) |
| `teacher_id` | INTEGER | FK → teachers(id) |
| `subject_id` | INTEGER | FK → subjects(id) |
| `grade_level` | VARCHAR(50) | NOT NULL |
| `paralelo` | VARCHAR(10) | NOT NULL |
| `date` | DATE | NOT NULL |
| `status` | VARCHAR(10) | CHECK IN ('presente','tarde','falta') |
| `created_at` | TIMESTAMPTZ | DEFAULT NOW() |
| — | — | UNIQUE(student_id, subject_id, date) |

### `courses`

| Columna | Tipo | Restricciones |
|---|---|---|
| `id` | SERIAL | PK |
| `grade_level` | VARCHAR(50) | NOT NULL |
| `paralelo` | VARCHAR(10) | NOT NULL |
| `max_students` | INTEGER | NOT NULL |
| `min_students_to_open` | INTEGER | NOT NULL |

## 9.3 Relaciones

- `users.student_id` → `students.id` (1:1)
- `users.teacher_id` → `teachers.id` (1:1)
- `users.parent_of_student_id` → `students.id` (1:1)
- `teacher_assignments.teacher_id` → `teachers.id` (N:1)
- `teacher_assignments.subject_id` → `subjects.id` (N:1)
- `grades.student_id` → `students.id` (N:1)
- `grades.subject_id` → `subjects.id` (N:1)
- `grades.teacher_id` → `teachers.id` (N:1)
- `attendance.student_id` → `students.id` (N:1)
- `attendance.subject_id` → `subjects.id` (N:1)
- `attendance.teacher_id` → `teachers.id` (N:1)

## 9.4 Índices recomendados

```sql
CREATE UNIQUE INDEX idx_users_email ON users(email);
CREATE UNIQUE INDEX idx_students_ci ON students(ci);
CREATE UNIQUE INDEX idx_students_rude ON students(rude);
CREATE UNIQUE INDEX idx_teachers_ci ON teachers(ci);
CREATE INDEX idx_grades_student_period ON grades(student_id, period);
CREATE INDEX idx_attendance_student_date ON attendance(student_id, date);
CREATE INDEX idx_teacher_assignments_teacher ON teacher_assignments(teacher_id);
```

## 9.5 Validaciones de dominio

Las validaciones se ejecutan en la capa de dominio (`backend/src/domain/validators/PersonValidator.ts`).

| Campo | Regla |
|---|---|
| Nombres y apellidos | 3–50 chars, solo letras (acentos y ñ), mayúscula inicial por palabra, al menos una vocal, máximo 3 consonantes seguidas, sin palabras repetidas |
| CI | 6–8 dígitos numéricos |
| RUDE | 6–20 dígitos numéricos |
| Teléfono | 8 dígitos, comienza con 6 o 7 |
| Email | Formato válido, ≤100 chars |
| Fecha de nacimiento | ≥1950 y ≤ hoy |

## 9.6 Zona horaria

La base de datos se configura en `America/La_Paz`. Los timestamps se almacenan en formato local sin milisegundos.