# 05 — Historias de Usuario

## 5.1 Rol: Administrador (`admin`)

### HU-01 — Iniciar sesión

Como administrador, quiero iniciar sesión con mi cuenta de Google, para acceder al sistema de forma segura.

Criterios de aceptación:

- El botón "Iniciar sesión con Google" redirige al flujo OAuth.
- Si mi email no está registrado → mensaje "El correo no está registrado en el sistema".
- Si mi cuenta está desactivada → mensaje "Tu usuario está desactivado".
- Si el login es exitoso → redirección al dashboard de administrador.

### HU-02 — Registrar estudiante

Como administrador, quiero registrar un estudiante con sus datos personales y de tutor, para incorporarlo al sistema.

Criterios de aceptación:

- CI válido (6–8 dígitos) y único.
- RUDE válido (6–20 dígitos) y único.
- Nombres y apellidos con mayúscula inicial y mínimo 3 caracteres.
- Teléfono del tutor de 8 dígitos, comienza con 6 o 7.
- Edad corresponde al curso seleccionado.
- Si hay duplicado → mensaje específico.

### HU-03 — Editar estudiante

Como administrador, quiero editar los datos de un estudiante, para corregir información.

### HU-04 — Listar estudiantes

Como administrador, quiero ver la lista de estudiantes, para consultar su información.

### HU-05 — Registrar docente

Como administrador, quiero registrar un docente con su especialidad y fecha de contratación.

### HU-06 — Gestionar materias

Como administrador, quiero crear, editar y eliminar materias por nivel educativo.

### HU-07 — Asignar docente a materia/curso

Como administrador, quiero asignar un docente a una materia y curso específico.

### HU-08 — Registrar notas

Como administrador, quiero registrar las notas de un estudiante por trimestre.

### HU-09 — Registrar asistencia

Como administrador, quiero registrar la asistencia diaria de un estudiante.

## 5.2 Rol: Docente (`teacher`)

### HU-10 — Registrar notas de sus cursos

Como docente, quiero registrar las notas de los estudiantes de mis cursos asignados.

### HU-11 — Registrar asistencia de sus cursos

Como docente, quiero registrar la asistencia diaria de mis cursos.

### HU-12 — Ver dashboard docente

Como docente, quiero ver un dashboard con mis cursos y asignaciones.

## 5.3 Rol: Estudiante (`student`)

### HU-13 — Ver mi perfil

Como estudiante, quiero ver mi información personal en mi panel.

### HU-14 — Ver mis materias

Como estudiante, quiero ver las materias que curso.

### HU-15 — Ver mis notas

Como estudiante, quiero consultar mis calificaciones por trimestre.

### HU-16 — Ver mi asistencia

Como estudiante, quiero consultar mi asistencia diaria.

## 5.4 Rol: Padre/Tutor (`parent`)

### HU-17 — Ver dashboard de mi hijo

Como padre, quiero ver el resumen académico de mi hijo.

### HU-18 — Ver materias de mi hijo

Como padre, quiero consultar las materias que cursa mi hijo.

### HU-19 — Ver detalle de una materia

Como padre, quiero ver el detalle de notas y asistencia de una materia específica de mi hijo.

## 5.5 Matriz HU → RF

| HU | RF asociados |
|---|---|
| HU-01 | RF-01, RF-02 |
| HU-02 | RF-04 |
| HU-03 | RF-06 |
| HU-04 | RF-05 |
| HU-05 | RF-08, RF-09 |
| HU-06 | RF-10 a RF-13 |
| HU-07 | RF-14 a RF-16 |
| HU-08 | RF-17 a RF-20 |
| HU-09 | RF-21, RF-22 |
| HU-10 | RF-17 a RF-20 |
| HU-11 | RF-21, RF-22 |
| HU-12 | RF-25 |
| HU-13 a HU-16 | RF-23 |
| HU-17 a HU-19 | RF-24 |