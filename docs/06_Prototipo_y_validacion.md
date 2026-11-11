# 06 — Prototipo y Validación

## 6.1 Pantallas implementadas

| Pantalla | Archivo | Rol | Estado |
|---|---|---|---|
| Login | `LoginPage.tsx` | Público | Implementado |
| Registro de estudiante | `RegisterPage.tsx` | admin | Implementado |
| Dashboard principal | `DashboardPage.tsx` | admin | Implementado |
| Dashboard docente | `TeacherDashboardPage.tsx` | teacher | Implementado |
| Dashboard estudiante | `StudentDashboardPage.tsx` | student | Implementado |
| Dashboard padre | `ParentDashboardPage.tsx` | parent | Implementado |
| Lista de estudiantes | `StudentsListPage.tsx` | admin | Implementado |
| Gestión de estudiantes | `StudentsPage.tsx` | admin | Implementado |
| Lista de docentes | `TeachersListPage.tsx` | admin | Implementado |
| Gestión de docentes | `TeachersPage.tsx` | admin | Implementado |
| Lista genérica | `ListPage.tsx` | admin | Implementado |

## 6.2 Flujos de validación

### 6.2.1 Validación en el registro de estudiante

El formulario de registro aplica las siguientes validaciones en el cliente y en el servidor:

| Campo | Regla | Mensaje de error |
|---|---|---|
| Nombres | 3–50 chars, mayúscula inicial, solo letras | "Nombre inválido" |
| Apellido paterno | Igual que nombres | "Apellido inválido" |
| Apellido materno | Igual que nombres | "Apellido inválido" |
| CI | 6–8 dígitos, único | "CI ya existe, intente con otro" |
| RUDE | 6–20 dígitos, único | "Código de RUDE ya existente" |
| Teléfono tutor | 8 dígitos, empieza con 6 o 7 | "Solo número boliviano" |
| Email tutor | Formato válido | "Email inválido" |
| Fecha nacimiento | ≥1950 y ≤ hoy | "Fecha inválida" |
| Curso vs edad | Matriz de rangos por curso | "Edad no permitida" |

### 6.2.2 Validación de duplicados

El sistema detecta:

- CI duplicado: bloquea el registro.
- RUDE duplicado: bloquea el registro.
- Persona idéntica duplicada: si coinciden nombres y apellidos completos, bloquea.
- Hermanos: permite compartir apellidos y tutores, siempre que nombres de pila y CI/RUDE sean diferentes.

### 6.2.3 Validación de autenticación

| Escenario | Comportamiento |
|---|---|
| Token de Google inválido | 401 "Token de Google inválido" |
| Email no verificado en Google | 401 "El email de Google no está verificado" |
| Email no registrado en el sistema | 401 "El correo no está registrado en el sistema" |
| Usuario desactivado | 401 "Tu usuario está desactivado" |
| JWT expirado | 401 "Token inválido o expirado" |
| Sin header Authorization | 401 "No autorizado. Falta el token" |

## 6.3 Pruebas manuales realizadas

| # | Prueba | Resultado esperado | Estado |
|---|---|---|---|
| P1 | Login con cuenta autorizada | Redirección al dashboard | Exitoso |
| P2 | Login con cuenta no registrada | Mensaje de error | Exitoso |
| P3 | Registro de estudiante válido | Estudiante creado | Exitoso |
| P4 | Registro con CI duplicado | Error "CI ya existe" | Exitoso |
| P5 | Registro con edad fuera de rango | Error "Edad no permitida" | Exitoso |
| P6 | Acceso a endpoint sin token | 401 | Exitoso |
| P7 | Acceso a endpoint con token expirado | 401 | Exitoso |
| P8 | Auto-logout tras 10 min de inactividad | Cierre de sesión | Exitoso |