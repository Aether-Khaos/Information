# 02 — Requerimientos Funcionales

## 2.1 Tabla de requerimientos

| Código | Requisito | Endpoint backend | Pantalla frontend | Estado |
|---|---|---|---|---|
| RF-01 | Inicio de sesión con Google OAuth 2.0 | `POST /api/auth/google` | `LoginPage.tsx` | Implementado |
| RF-02 | Consultar perfil del usuario autenticado | `GET /api/auth/me` | `App.tsx` | Implementado |
| RF-03 | Auto-logout por inactividad (10 min) | — | `useAutoLogout.ts` | Implementado |
| RF-04 | Registrar estudiante con validaciones de dominio | `POST /api/students` | `RegisterPage.tsx` | Implementado |
| RF-05 | Listar estudiantes | `GET /api/students` | `StudentsListPage.tsx` | Implementado |
| RF-06 | Editar estudiante | `PUT /api/students/:id` | `StudentsPage.tsx` | Implementado |
| RF-07 | Listar docentes | `GET /api/teachers` | `TeachersListPage.tsx` | Implementado |
| RF-08 | Registrar docente | `POST /api/teachers` | `TeachersPage.tsx` | Implementado |
| RF-09 | Editar docente | `PUT /api/teachers/:id` | `TeachersPage.tsx` | Implementado |
| RF-10 | Listar materias | `GET /api/subjects` | — | Implementado |
| RF-11 | Crear materia | `POST /api/subjects` | — | Implementado |
| RF-12 | Editar materia | `PUT /api/subjects/:id` | — | Implementado |
| RF-13 | Eliminar materia | `DELETE /api/subjects/:id` | — | Implementado |
| RF-14 | Asignar docente a materia/curso | `POST /api/teacher-assignments` | — | Implementado |
| RF-15 | Listar asignaciones | `GET /api/teacher-assignments` | — | Implementado |
| RF-16 | Eliminar asignación | `DELETE /api/teacher-assignments/:id` | — | Implementado |
| RF-17 | Registrar nota trimestral | `POST /api/grades` | — | Implementado |
| RF-18 | Listar notas | `GET /api/grades` | — | Implementado |
| RF-19 | Editar nota | `PUT /api/grades/:id` | — | Implementado |
| RF-20 | Eliminar nota | `DELETE /api/grades/:id` | — | Implementado |
| RF-21 | Registrar asistencia | `POST /api/attendance` | — | Implementado |
| RF-22 | Listar asistencia | `GET /api/attendance` | — | Implementado |
| RF-23 | Panel del estudiante | `/api/student/me/*` | `StudentDashboardPage.tsx` | Implementado |
| RF-24 | Panel del padre | `/api/parent/me/*` | `ParentDashboardPage.tsx` | Implementado |
| RF-25 | Dashboard según rol | — | `DashboardPage.tsx`, `TeacherDashboardPage.tsx` | Implementado |

## 2.2 Detalle de requerimientos críticos

### RF-01 — Inicio de sesión con Google OAuth 2.0

Descripción: el usuario se autentica con su cuenta de Google. El backend verifica el `id_token`, comprueba que el email exista en la tabla `users` y que `is_active = true`. Si es válido, emite un JWT propio.

Criterios de aceptación:

- Si el email no está registrado → 401 con mensaje "El correo no está registrado en el sistema".
- Si `is_active = false` → 401 con mensaje "Tu usuario está desactivado".
- Si el `id_token` es inválido → 401 con mensaje "Token de Google inválido".
- Si todo es válido → 200 con `{ token, user }`.

Evidencia: `backend/src/application/use-cases/LoginWithGoogleUseCase.ts`

### RF-04 — Registro de estudiante con validaciones

Descripción: el administrador registra un estudiante. El backend valida:

- Nombres y apellidos: 3–50 chars, solo letras (acentos y ñ), mayúscula inicial por palabra, al menos una vocal, máximo 3 consonantes seguidas, sin palabras repetidas.
- CI: 6–8 dígitos numéricos, único en el sistema.
- RUDE: 6–20 dígitos numéricos, único en el sistema.
- Teléfono del tutor: 8 dígitos, comienza con 6 o 7.
- Email del tutor: formato válido, ≤100 chars.
- Fecha de nacimiento: ≥1950 y ≤ hoy.
- Edad por curso: rango permitido según curso escolar boliviano.

Criterios de aceptación:

- CI duplicado → 400 "CI ya existe, intente con otro".
- RUDE duplicado → 400 "Código de RUDE ya existente".
- Nombre inválido → 400 con detalle del campo.
- Edad fuera de rango → 400 "Edad no permitida".

Evidencia: `backend/src/domain/validators/PersonValidator.ts`

## 2.3 Trazabilidad RF → código

| RF | Archivo backend | Archivo frontend |
|---|---|---|
| RF-01, RF-02 | `LoginWithGoogleUseCase.ts`, `authRoutes.ts` | `LoginPage.tsx`, `App.tsx` |
| RF-03 | — | `useAutoLogout.ts` |
| RF-04 a RF-06 | `PersonValidator.ts`, `studentRoutes.ts` | `RegisterPage.tsx`, `StudentsPage.tsx`, `StudentsListPage.tsx` |
| RF-07 a RF-09 | `teacherRoutes.ts` | `TeachersPage.tsx`, `TeachersListPage.tsx` |
| RF-10 a RF-13 | `subjectRoutes.ts` | — |
| RF-14 a RF-16 | `teacherAssignmentRoutes.ts` | — |
| RF-17 a RF-20 | `gradeRoutes.ts` | — |
| RF-21, RF-22 | `attendanceRoutes.ts` | — |
| RF-23 | `studentPanelRoutes.ts` | `StudentDashboardPage.tsx` |
| RF-24 | `parentPanelRoutes.ts` | `ParentDashboardPage.tsx` |
| RF-25 | — | `DashboardPage.tsx`, `TeacherDashboardPage.tsx` |