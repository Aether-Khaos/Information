# 04 — Restricciones

## 4.1 Restricciones técnicas

| Código | Restricción | Impacto |
|---|---|---|
| RT-01 | Node.js 20+ requerido | Backend usa ESM nativo y `tsx` |
| RT-02 | PostgreSQL 15+ | Uso de `gen_random_uuid()` y tipos `jsonb` |
| RT-03 | Google Cloud OAuth Client obligatorio | Sin `GOOGLE_CLIENT_ID` no hay login |
| RT-04 | Conexión SSL obligatoria a BD en producción | `ssl: { rejectUnauthorized: false }` |
| RT-05 | Frontend SPA (sin SSR) | No apto para SEO de contenido público |

## 4.2 Restricciones de negocio

| Código | Restricción | Impacto |
|---|---|---|
| RN-01 | CI único por estudiante | No se permiten duplicados |
| RN-02 | RUDE único por estudiante | No se permiten duplicados |
| RN-03 | Teléfono boliviano (8 dígitos, empieza con 6 o 7) | No acepta números internacionales |
| RN-04 | Edad del estudiante debe corresponder al curso | Matriz de rangos por curso escolar |
| RN-05 | Un estudiante puede tener hermanos registrados | Se permite compartir apellidos y tutores, pero no identidad completa |
| RN-06 | Notas por trimestre: SER, SABER, HACER, DECIDIR, Autoevaluación | Total calculado |
| RN-07 | Asistencia por día, materia y estudiante | Único por combinación |

## 4.3 Restricciones de despliegue

| Código | Restricción | Impacto |
|---|---|---|
| RD-01 | Frontend desplegado en plataforma serverless | Sin procesos persistentes |
| RD-02 | Backend desplegado en plataforma con cold starts | Primera petición puede tardar |
| RD-03 | BD serverless con límite de conexiones | Pool con `max` acotado |
| RD-04 | Sin CI/CD automatizado | Despliegue manual |

## 4.4 Restricciones de alcance

- No se implementa gestión de contraseñas locales.
- No se implementan notificaciones push ni email transaccional.
- No se implementa auditoría de cambios.
- No se implementan reportes exportables (PDF/Excel).
- No se implementa aplicación móvil nativa.