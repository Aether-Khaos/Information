# 13 — Reporte de Entrega Final

## 13.1 Estado del producto

El Sistema Académico se encuentra funcional y desplegado en producción. Cubre los módulos de autenticación, gestión de estudiantes, docentes, materias, asignaciones, notas, asistencia y paneles por rol.

## 13.2 Módulos implementados

| Módulo | Estado |
|---|---|
| Autenticación OAuth 2.0 + JWT | Completo |
| Auto-logout por inactividad | Completo |
| CRUD de estudiantes con validaciones | Completo |
| CRUD de docentes | Completo |
| CRUD de materias | Completo |
| Asignaciones docente–materia–curso | Completo |
| Registro de notas trimestrales | Completo |
| Registro de asistencia | Completo |
| Panel del estudiante | Completo |
| Panel del padre | Completo |
| Dashboard del docente | Completo |
| Dashboard del administrador | Completo |

## 13.3 Requerimientos cumplidos

- 25 de 25 requerimientos funcionales implementados.
- 10 de 12 requerimientos no funcionales implementados.
- 2 RNF pendientes: tests automatizados y migraciones versionadas.

## 13.4 Deuda técnica identificada

| # | Deuda | Impacto | Prioridad |
|---|---|---|---|
| D1 | `requireRole` definido pero no aplicado en rutas | Alto | Alta |
| D2 | Swagger sin endpoints documentados (`paths: {}`) | Medio | Alta |
| D3 | Sin migraciones versionadas | Medio | Media |
| D4 | Sin tests automatizados | Medio | Media |
| D5 | Sin CI/CD | Bajo | Baja |
| D6 | CORS abierto (`origin: '*'`) | Medio | Media |
| D7 | `JWT_SECRET` con fallback en código | Alto | Alta |

## 13.5 Plan de trabajo futuro

| Prioridad | Tarea | Esfuerzo |
|---|---|---|
| Alta | Aplicar `requireRole` en rutas de escritura | 10 min |
| Alta | Documentar los 29 endpoints en Swagger | 1–2 h |
| Alta | Eliminar fallback de `JWT_SECRET` | 15 min |
| Media | Restringir CORS al dominio del frontend | 5 min |
| Media | Añadir migraciones versionadas | 2–3 h |
| Media | Tests de dominio con Vitest | 1–2 h |
| Media | Tests de integración con Supertest | 3–4 h |
| Baja | CI/CD con GitHub Actions | 2–3 h |
| Baja | Refresh tokens | 4–6 h |
| Baja | Notificaciones email | 3–4 h |
| Baja | Auditoría de cambios | 2–3 h |

## 13.6 Lecciones aprendidas

- La delegación de autenticación a un proveedor externo (Google) simplifica la capa de seguridad y es más robusta que la gestión local de contraseñas.
- La separación estricta de capas permite cambiar la BD o el framework HTTP sin tocar la lógica de negocio.
- La documentación temprana de reglas de negocio es tan valiosa como el código mismo.
- La ausencia de migraciones versionadas y tests automatizados es una deuda que debe saldarse antes de escalar el sistema.

## 13.7 Conclusión

El Sistema Académico cumple con los objetivos planteados en la primera entrega y se encuentra operativo en producción. La arquitectura Clean Architecture, la autenticación OAuth 2.0 + JWT y las validaciones de dominio específicas del contexto boliviano constituyen los pilares del sistema. La deuda técnica identificada está mapeada y priorizada en el plan de trabajo futuro.