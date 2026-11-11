# 03 — Requerimientos No Funcionales

## 3.1 Seguridad

| Código | Requisito | Implementación | Estado |
|---|---|---|---|
| RNF-S01 | Autenticación delegada a proveedor OAuth 2.0 | `google-auth-library` → `verifyIdToken` | Implementado |
| RNF-S02 | Emisión de JWT firmado con expiración configurable | `jsonwebtoken`, `JWT_EXPIRES_IN` (default `7d`) | Implementado |
| RNF-S03 | Protección de rutas con middleware JWT | `middleware/requireAuth.ts` | Implementado |
| RNF-S04 | Control de acceso por roles | `middleware/requireRole.ts` | Definido, pendiente de aplicar en rutas |
| RNF-S05 | Validación de entrada en capa de dominio | `domain/validators/PersonValidator.ts` | Implementado |
| RNF-S06 | Auto-logout por inactividad | `presentation/hooks/useAutoLogout.ts` (10 min) | Implementado |
| RNF-S07 | Conexión cifrada a la BD | `ssl: { rejectUnauthorized: false }` en el Pool | Implementado |

## 3.2 Rendimiento

| Código | Requisito | Implementación | Estado |
|---|---|---|---|
| RNF-P01 | Pool de conexiones reutilizable | `new Pool(...)` en `infrastructure/database/index.ts` | Implementado |
| RNF-P02 | Índices en columnas de búsqueda frecuente | Recomendados en `09_Modelo_de_Datos.md` | Pendiente de aplicar |
| RNF-P03 | Respuestas < 150 ms en consultas estándar | No medido formalmente | No verificado |

## 3.3 Usabilidad

| Código | Requisito | Implementación | Estado |
|---|---|---|---|
| RNF-U01 | Interfaz responsiva | Tailwind CSS v4 en todas las páginas | Implementado |
| RNF-U02 | Mensajes de error claros | Mensajes específicos por campo en validaciones | Implementado |
| RNF-U03 | Carga inicial rápida | Vite como bundler, code splitting nativo | Implementado |
| RNF-U04 | Paneles diferenciados por rol | 4 dashboards distintos | Implementado |

## 3.4 Mantenibilidad

| Código | Requisito | Implementación | Estado |
|---|---|---|---|
| RNF-M01 | Separación de capas | Clean Architecture en backend y frontend | Implementado |
| RNF-M02 | Tipado estático | TypeScript en todo el proyecto | Implementado |
| RNF-M03 | Configuración por variables de entorno | `dotenv` en backend, `VITE_*` en frontend | Implementado |
| RNF-M04 | Linter configurado | `oxlint` en frontend | Implementado |
| RNF-M05 | Tests automatizados | No implementado | Pendiente |
| RNF-M06 | Migraciones versionadas | No implementado | Pendiente |

## 3.5 Compatibilidad

| Código | Requisito | Implementación |
|---|---|---|
| RNF-C01 | Node.js 20+ | Definido en despliegue |
| RNF-C02 | Navegadores modernos | React 19 + Vite (ES2022+) |
| RNF-C03 | PostgreSQL 15+ | Docker local y nube |

## 3.6 Justificación de OAuth en lugar de hash local

El sistema no almacena contraseñas. La verificación de identidad se delega a Google Identity Platform, que aplica hashing robusto (bcrypt/scrypt con salt) en sus propios servidores. Esto elimina la superficie de ataque de contraseñas locales (credential stuffing, rainbow tables, filtraciones de BD), cumple y supera el requisito de "hash robusto con salt", trasladándolo a un proveedor certificado, y es el estándar recomendado por OWASP para aplicaciones modernas.

La seguridad propia del sistema se concentra en: validar el `id_token` de Google, verificar que el email esté autorizado en la tabla `users`, emitir un JWT propio firmado y proteger cada endpoint con `requireAuth`.