# 01 — Introducción y Contexto

## 1.1 Contexto del proyecto

El Sistema Académico nace de la necesidad de digitalizar la gestión escolar de un colegio boliviano que gestionaba la información académica en hojas de cálculo y cuadernos físicos. Esto generaba duplicidad y pérdida de registros, errores en el cálculo de notas por trimestre, dificultad para que padres y estudiantes consultaran calificaciones y asistencia, y ausencia de control de acceso por rol.

## 1.2 Objetivo general

Desarrollar una aplicación web fullstack que centralice la gestión académica de un colegio, con autenticación segura, control de acceso por roles y paneles personalizados para administradores, docentes, estudiantes y padres.

## 1.3 Objetivos específicos

1. Implementar autenticación delegada a un proveedor OAuth 2.0 con emisión de JWT propio.
2. Modelar el dominio académico boliviano con validaciones específicas (CI, RUDE, teléfono, edades por curso).
3. Proveer CRUD de estudiantes, docentes, materias, asignaciones, notas y asistencia.
4. Ofrecer paneles de solo lectura para estudiantes y padres.
5. Desplegar el sistema en un entorno de producción accesible.

## 1.4 Alcance funcional

El sistema cubre los siguientes módulos:

- Autenticación: login con Google, sesión JWT, auto-logout por inactividad.
- Estudiantes: registro con validaciones de dominio, listado, edición.
- Docentes: registro con especialidad, listado, edición.
- Materias: CRUD por nivel educativo.
- Asignaciones: vínculo docente–materia–curso.
- Notas: registro trimestral por componentes (SER, SABER, HACER, DECIDIR, Autoevaluación).
- Asistencia: registro diario por estado (presente, tarde, falta).
- Paneles: dashboards diferenciados por rol.

## 1.5 Fuera de alcance

- Gestión de contraseñas locales (delegado a Google).
- Notificaciones push o email transaccional.
- Auditoría de cambios (`updated_by`, `deleted_at`).
- Reportes exportables (PDF/Excel).
- Facturación o gestión financiera.
- Aplicación móvil nativa.

## 1.6 Usuarios del sistema

| Rol | Descripción |
|---|---|
| `admin` | Dirección / secretaría académica. CRUD completo. |
| `teacher` | Docente. Registra notas y asistencia de sus cursos. |
| `student` | Estudiante. Consulta su panel (solo lectura). |
| `parent` | Padre/tutor. Consulta el panel de su hijo (solo lectura). |
| `staff` | Personal administrativo. Reservado para futuras expansiones. |