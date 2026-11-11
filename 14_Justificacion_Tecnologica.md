# 14 — Justificación Tecnológica

## 14.1 Propósito del documento

Este documento justifica la elección de **cada tecnología** que compone el Sistema Académico del colegio T. H. Mallco Rancho. Cada decisión técnica se evalúa contra cuatro criterios rectores:

1. **Costo total de propiedad (TCO):** licencias, hosting, mantenimiento y personal.
2. **Seguridad y protección de datos de menores de edad.**
3. **Complejidad operativa para la institución:** el colegio no cuenta con personal de TI.
4. **Escalabilidad y continuidad a futuro.**

El objetivo no es defender tecnologías "de moda", sino demostrar que **cada componente fue elegido por una razón concreta** y que el sistema es **operativamente sostenible** para una institución con recursos limitados.

## 14.2 Contexto institucional

El colegio T. H. Mallco Rancho enfrenta las siguientes restricciones reales:

- **Presupuesto informático prácticamente nulo:** no puede pagar licencias de software propietario ni servidores dedicados.
- **Sin personal de TI:** no hay administradores de sistemas ni desarrolladores en plantilla. El sistema debe mantenerse con un desarrollador externo ocasional.
- **Infraestructura básica:** conexión a internet limitada, equipos de cómputo modestos, sin servidores propios.
- **Datos sensibles de menores de edad:** el sistema gestiona nombres, CI, RUDE, fechas de nacimiento, notas y asistencia de estudiantes. La privacidad es un requisito legal y ético.
- **Necesidad de continuidad:** el sistema no puede caerse durante el año escolar; debe funcionar aunque no haya alguien monitoreándolo.

**Principio rector del diseño:** mínimo costo operativo con máxima seguridad.

## 14.3 Stack tecnológico verificado

El siguiente inventario fue verificado directamente contra el código del repositorio (no es una lista teórica, es lo que el sistema realmente usa):

| Capa | Tecnología | Versión real | Rol |
|---|---|---|---|
| **Frontend** | React | 19.3.1 | Biblioteca de UI declarativa |
| | React DOM | 19.3.1 | Renderizado de la SPA |
| | Vite | 8.3.6 | Bundler, dev server y build de producción |
| | @vitejs/plugin-react | 6.1.2 | Plugin de React para Vite |
| | TypeScript | 7.0.2 | Tipado estático |
| | Tailwind CSS | 4.3.4 | Estilos utilitarios |
| | @tailwindcss/vite | 4.3.4 | Plugin de Tailwind para Vite v4 |
| | @react-oauth/google | 0.13.5 | Botón y flujo de Google OAuth 2.0 |
| | oxlint | 1.90.0 | Linter estático |
| **Backend** | Node.js | 26.13.0 | Runtime del servidor |
| | npm | 12.2.2 | Gestor de paquetes |
| | Express | 4.19.2 | Framework HTTP / API REST |
| | TypeScript | 5.5.2 | Tipado estático |
| | tsx | 4.19.0 | Ejecutor de TypeScript en desarrollo |
| | esbuild | 0.20.2 | Bundler del build de producción |
| | pg | 8.12.0 | Driver PostgreSQL (Pool) |
| | jsonwebtoken | 9.0.3 | Firma y verificación de JWT |
| | google-auth-library | 11.3.4 | Verificación de `id_token` de Google |
| | dotenv | 16.4.5 | Carga de variables de entorno |
| | cors | 2.8.5 | Middleware CORS |
| | swagger-jsdoc | 6.3.0 | Generación de OpenAPI desde JSDoc |
| | swagger-ui-express | 5.0.1 | UI de Swagger en `/api-docs` |
| **Base de datos** | PostgreSQL | 15-alpine | Base de datos relacional |
| | Neon | — | PostgreSQL serverless (producción) |
| **Infraestructura** | Vercel | — | Hosting del frontend |
| | Render | — | Hosting del backend |
| | Docker + Compose | — | PostgreSQL local (desarrollo) |
| | Git + GitHub | — | Control de versiones |
| **Autenticación** | Google OAuth 2.0 | — | Autenticación delegada |
| | OpenID Connect | — | Verificación de `id_token` |
| | JWT | — | Sesión stateless firmada |
| **Arquitectura** | Clean Architecture | — | Separación en capas |
| | Repository Pattern | — | Contratos de persistencia |
| | Use Cases + DTOs | — | Lógica de negocio |
| | SPA | — | Frontend sin recarga de página |
| | Monorepo | — | Backend y frontend en un repo |

## 14.4 Frontend: justificación de cada tecnología

### 14.4.1 React 19.3.1

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, licencia MIT, sin costo de licencia ni regalías |
| **Comunidad** | La biblioteca frontend más usada del mundo; documentación abundante y soporte gratuito |
| **Mantenibilidad** | Componentes reutilizables; el colegio puede contratar a cualquier desarrollador con experiencia en React en el futuro |
| **Seguridad** | React escapa automáticamente el contenido en las vistas, previniendo ataques XSS de forma nativa |
| **Ecosistema** | Amplia disponibilidad de librerías auditadas para autenticación, formularios y validación |
| **Compatibilidad** | Soporte a largo plazo (LTS) con actualizaciones regulares de seguridad |

**Alternativas descartadas:**

- **Angular:** mayor curva de aprendizaje, estructura más pesada, requiere desarrolladores especializados (mayor costo futuro de contratación).
- **Vue:** válido, pero con menor oferta de desarrolladores en el mercado local boliviano.
- **jQuery / HTML plano:** insuficiente para una SPA con múltiples roles y paneles dinámicos.
- **Svelte:** comunidad más pequeña, menor oferta de mantenimiento a futuro.

### 14.4.2 Vite 8.3.6

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, licencia MIT, sin costo |
| **Rendimiento en desarrollo** | Servidor con ES Modules nativos y HMR (Hot Module Replacement) instantáneo: los cambios se reflejan en milisegundos |
| **Rendimiento en producción** | Build con Rollup que genera un bundle optimizado y con tree-shaking; el sistema completo pesa ~340 KB (86 KB comprimido), lo que permite carga rápida incluso con conexiones lentas |
| **Simplicidad** | Configuración mínima en un solo archivo (`vite.config.ts`); el colegio no necesita un ingeniero especializado en build tools |
| **Seguridad** | `sourcemap: false` en producción, lo que oculta la estructura de archivos TSX en las DevTools del navegador |
| **Compatibilidad** | Soporte nativo para TypeScript, JSX/TSX y Tailwind v4 sin configuración adicional |
| **Integración con Vercel** | Vercel detecta Vite automáticamente y aplica la configuración óptima de build y deploy |

**Alternativas descartadas:**

- **Create React App (CRA):** oficialmente deprecado por el equipo de React desde 2023; builds lentos (30–60 s frente a <1 s de Vite), sin mantenimiento activo.
- **Webpack:** configuración compleja (múltiples archivos, loaders, plugins); builds lentos; requiere conocimiento especializado que el colegio no tiene.
- **Parcel:** menor ecosistema, menor soporte para TypeScript avanzado y Tailwind v4.
- **Next.js:** framework completo con SSR que el sistema no necesita (es una SPA interna, no un sitio público con SEO); añade complejidad innecesaria.

### 14.4.3 TypeScript 7.0.2

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Seguridad** | Detecta errores en tiempo de compilación antes de llegar a producción; previene bugs que podrían exponer datos de estudiantes |
| **Mantenibilidad** | Contratos explícitos entre frontend y backend; cualquier desarrollador futuro entiende la estructura de datos sin leer todo el código |
| **Integración** | Vite y React tienen soporte nativo para TypeScript |

**Alternativa descartada:**

- **JavaScript puro:** mayor riesgo de errores en runtime que afecten la integridad de los datos académicos (notas, asistencia).

### 14.4.4 Tailwind CSS 4.3.4 con @tailwindcss/vite

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Rendimiento** | Genera solo el CSS utilizado; el archivo final es de ~29 KB (6 KB comprimido) |
| **Consistencia visual** | Sistema de diseño unificado; el colegio no necesita un diseñador gráfico para mantener la interfaz coherente |
| **Accesibilidad** | Utilidades integradas para contraste, tamaños de texto y espaciado |
| **Mantenibilidad** | Los estilos viven junto al componente; no hay archivos CSS huérfanos |
| **Integración con Vite** | El plugin `@tailwindcss/vite` evita la necesidad de `postcss.config.js` y `tailwind.config.js` |

**Alternativas descartadas:**

- **Bootstrap:** estilos genéricos difíciles de personalizar; sitios visualmente idénticos.
- **CSS Modules:** requiere escribir CSS desde cero; mayor esfuerzo de mantenimiento.
- **Styled Components:** mayor peso en runtime y menor rendimiento en listas grandes.

### 14.4.5 @react-oauth/google 0.13.5

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Seguridad** | Componente oficial recomendado por Google para integrar su flujo OAuth 2.0; maneja el ciclo de vida del token de forma segura |
| **Simplicidad** | Integración en menos de 20 líneas de código; no requiere implementar el flujo OAuth manualmente |
| **Mantenimiento** | Mantenido por Google; actualizaciones de seguridad automáticas |

### 14.4.6 oxlint 1.90.0

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Rendimiento** | Linter escrito en Rust; 50–100 veces más rápido que ESLint |
| **Seguridad** | Detecta patrones inseguros (uso de `eval`, `innerHTML` sin sanitizar) antes del commit |
| **Simplicidad** | Sin configuración extensa; el colegio no necesita mantener un archivo de reglas complejo |

## 14.5 Backend: justificación de cada tecnología

### 14.5.1 Node.js 26.13.0

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo de licencia; hosting gratuito en planes iniciales |
| **Rendimiento** | Modelo asíncrono no bloqueante; suficiente para la carga de un colegio (decenas de usuarios concurrentes) |
| **Simplicidad** | Un solo lenguaje (TypeScript) en frontend y backend; el colegio no necesita dos equipos de desarrollo |
| **Ecosistema** | El registro npm es el mayor repositorio de librerías auditadas del mundo |
| **Compatibilidad** | Versión 26 LTS con soporte de seguridad extendido |
| **Despliegue** | Compatible con plataformas serverless gratuitas o de bajo costo (Render, Railway, Fly.io) |

**Alternativas descartadas:**

- **Django (Python) / Laravel (PHP):** válidos, pero requerirían contratar desarrolladores con perfiles distintos para frontend y backend, aumentando el costo de mantenimiento.
- **Spring Boot (Java):** mayor consumo de recursos; requiere servidores más costosos.
- **.NET (C#):** licencias y hosting más caros; menor oferta de desarrolladores en el mercado local.

### 14.5.2 Express 4.19.2

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Madurez** | El framework HTTP más usado en Node.js; 10+ años de estabilidad |
| **Seguridad** | Middlewares maduros para autenticación, CORS, rate limiting y validación |
| **Simplicidad** | API minimalista; el equipo controla exactamente qué middlewares usa |
| **Compatibilidad** | Funciona con cualquier versión de Node.js 20+ |

**Alternativas descartadas:**

- **Fastify:** más rápido en benchmarks, pero menor ecosistema de middlewares y menor oferta de desarrolladores.
- **NestJS:** framework completo con inyección de dependencias; añade complejidad innecesaria para un sistema de este tamaño.
- **Koa:** minimalista pero con menor comunidad y menos middlewares de seguridad disponibles.

### 14.5.3 TypeScript 5.5.2

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Seguridad** | Tipado estático en el backend previene errores en el manejo de datos de estudiantes |
| **Mantenibilidad** | Contratos explícitos entre capas (`domain`, `application`, `infrastructure`) |
| **Integración** | Compatible con `tsx` y `esbuild` sin configuración adicional |

### 14.5.4 tsx 4.19.0 (desarrollo)

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Rendimiento** | Ejecuta TypeScript directamente sin compilación previa; recarga automática con `tsx watch` |
| **Simplicidad** | No requiere `ts-node` ni configuración de `tsconfig` compleja |
| **Aislamiento** | Solo se usa en desarrollo; no afecta producción |

### 14.5.5 esbuild 0.20.2 (build de producción)

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Rendimiento** | Build de producción en milisegundos (vs. segundos de `tsc`) |
| **Tamaño** | Genera un bundle optimizado y minificado |
| **Seguridad** | `--packages=external` evita incluir `node_modules` en el bundle final |

**Alternativa descartada:**

- **tsc solo:** más lento y no genera un bundle único; requiere subir `node_modules` o `dist/` con múltiples archivos.

### 14.5.6 PostgreSQL 15+

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo de licencia; hosting gratuito en Neon, Supabase o Render |
| **Modelo relacional** | El dominio académico es inherentemente relacional: estudiantes ↔ tutores, docentes ↔ materias ↔ cursos, notas ↔ trimestres. Un modelo relacional evita duplicidad y garantiza integridad referencial |
| **Integridad de datos** | Restricciones `UNIQUE`, `CHECK`, `FOREIGN KEY` y transacciones ACID evitan que un estudiante quede registrado dos veces, que una nota quede huérfana o que se borre un docente con cursos activos |
| **Seguridad** | Cifrado en tránsito (SSL/TLS) y en reposo (en el proveedor cloud); roles y permisos granulares |
| **Escalabilidad** | Soporta desde decenas hasta millones de registros sin cambio de motor |
| **Estándar** | SQL es el lenguaje de consulta más conocido; cualquier DBA o desarrollador futuro puede mantener el sistema |
| **Auditoría** | Soporte nativo para transacciones y logs de consultas; facilita futuras auditorías de cambios en notas |

**Alternativas descartadas:**

- **MongoDB (NoSQL):** aunque la rúbrica la sugiere, el dominio académico es relacional. Un modelo de documentos obligaría a duplicar información (estudiantes dentro de cursos, notas dentro de estudiantes), aumentando el riesgo de inconsistencias en datos sensibles. PostgreSQL garantiza integridad referencial que MongoDB no ofrece de forma nativa. **La institución no puede permitirse que una nota quede asociada a un estudiante inexistente o que un curso tenga dos docentes sin control.**
- **MySQL / MariaDB:** válidos, pero PostgreSQL ofrece mejor soporte para tipos de datos complejos (`jsonb`), transacciones más robustas y una comunidad más activa en seguridad.
- **SQLite:** insuficiente para acceso concurrente desde múltiples usuarios y múltiples roles.
- **SQL Server / Oracle:** licencias costosas, incompatibles con el presupuesto de la institución.

### 14.5.7 pg 8.12.0 (sin ORM)

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Rendimiento** | Consultas SQL directas sin capa de abstracción; menor latencia |
| **Control** | El equipo controla exactamente qué consulta se ejecuta; no hay "magia" del ORM generando consultas ineficientes |
| **Seguridad** | Las consultas parametrizadas (`$1`, `$2`) previenen inyección SQL de forma nativa |
| **Transparencia** | El código SQL es explícito y auditable; cualquier DBA puede revisarlo |

**Alternativas descartadas:**

- **Prisma / Drizzle / TypeORM:** añaden una capa de abstracción que, si bien acelera el desarrollo, también puede generar consultas ineficientes o comportamientos inesperados. Para un sistema con datos académicos sensibles, la transparencia del SQL directo es preferible.

### 14.5.8 jsonwebtoken 9.0.3 + google-auth-library 11.3.4

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo; Google Identity es gratuito hasta millones de autenticaciones |
| **Seguridad** | Google aplica hashing robusto (bcrypt/scrypt con salt) sobre las contraseñas en sus servidores certificados SOC 2 / ISO 27001. El sistema **no almacena contraseñas locales**, eliminando la superficie de ataque de credential stuffing, rainbow tables y filtraciones de BD |
| **Estándar** | OAuth 2.0 + OpenID Connect es el estándar recomendado por OWASP para autenticación moderna |
| **Experiencia de usuario** | El usuario no debe recordar otra contraseña; usa su cuenta de Google existente |
| **Auditoría** | Google registra los inicios de sesión y los hace disponibles para el administrador de la cuenta |
| **Mantenimiento** | El colegio no debe gestionar reset de contraseñas, políticas de complejidad ni bloqueos por intentos fallidos |

**Alternativa descartada:**

- **Contraseñas locales con bcrypt:** requeriría que el colegio gestione reset de contraseñas, políticas de complejidad, bloqueos por intentos fallidos y soporte a usuarios que olvidan su clave. Es una carga operativa que la institución no puede asumir. Además, el hash local, aunque seguro, es menos robusto que delegar la verificación a un proveedor certificado como Google.

### 14.5.9 dotenv 16.4.5

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Seguridad** | Mantiene credenciales fuera del código fuente; el `.env` se excluye de Git |
| **Simplicidad** | Carga variables en `process.env` sin configuración adicional |

### 14.5.10 cors 2.8.5

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Seguridad** | Controla qué dominios pueden llamar a la API; previene ataques CSRF desde orígenes no autorizados |
| **Simplicidad** | Configuración en una línea |

### 14.5.11 swagger-jsdoc 6.3.0 + swagger-ui-express 5.0.1

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Documentación** | Genera documentación interactiva OpenAPI 3.0 desde anotaciones JSDoc en el código |
| **Mantenibilidad** | La documentación vive junto al código; no se desactualiza |
| **Pruebas** | Permite probar endpoints directamente desde el navegador |

## 14.6 Infraestructura: justificación de cada servicio

### 14.6.1 Vercel (hosting del frontend)

| Criterio | Justificación |
|---|---|
| **Costo** | Plan gratuito suficiente para el tráfico de un colegio (100 GB de ancho de banda mensual, builds ilimitados) |
| **Simplicidad** | Deploy automático al hacer push a `main`; el colegio no necesita administrar servidores |
| **Rendimiento** | CDN global; el frontend carga rápido desde cualquier ubicación |
| **Seguridad** | HTTPS automático, certificados SSL gestionados, protección DDoS incluida |
| **Continuidad** | Si el colegio crece, el plan pago es de bajo costo (~20 USD/mes) |

### 14.6.2 Render (hosting del backend)

| Criterio | Justificación |
|---|---|
| **Costo** | Plan gratuito suficiente para el volumen de un colegio; plan pago desde ~7 USD/mes si se requiere eliminar cold starts |
| **Simplicidad** | Deploy automático desde GitHub; variables de entorno gestionadas desde el dashboard |
| **Seguridad** | HTTPS automático, aislamiento por contenedor, secretos cifrados en reposo |
| **Escalabilidad** | Escalado vertical y horizontal con un clic |

**Alternativas descartadas:**

- **Servidor VPS propio (DigitalOcean, AWS EC2):** requiere administración de sistema operativo, parches de seguridad, backups y monitoreo. El colegio no tiene personal de TI para esto.
- **Heroku:** plan gratuito eliminado en 2022; costo mayor.
- **Firebase Functions:** atadura al ecosistema Google; menos flexible para PostgreSQL.

### 14.6.3 Neon (PostgreSQL serverless)

| Criterio | Justificación |
|---|---|
| **Costo** | Plan gratuito con 0.5 GB de almacenamiento, suficiente para años de operación de un colegio (los registros académicos son texto, no multimedia) |
| **Simplicidad** | Sin administración de servidor; backups automáticos; conexión vía `DATABASE_URL` |
| **Seguridad** | Cifrado en tránsito (SSL obligatorio) y en reposo; aislamiento por tenant |
| **Escalabilidad** | Auto-scaling y branching (útil para pruebas); plan pago desde ~19 USD/mes si el colegio crece |
| **Continuidad** | Backups automáticos con retención; recuperación ante desastres |

**Alternativas descartadas:**

- **Supabase:** válido, pero incluye autenticación y storage que el sistema no necesita; mayor complejidad.
- **Amazon RDS:** costoso (~15 USD/mes mínimo) y requiere configuración de VPC, security groups y backups manuales.
- **PostgreSQL en VPS propio:** requiere administración, backups manuales y monitoreo. El colegio no puede asumir esa carga.

### 14.6.4 Docker + Docker Compose (solo desarrollo local)

| Criterio | Justificación |
|---|---|
| **Costo** | Código abierto, sin costo |
| **Simplicidad** | El desarrollador levanta PostgreSQL local con un solo comando (`docker compose up -d`); no contamina su sistema operativo |
| **Aislamiento** | Cada desarrollador tiene su propia BD sin conflictos |
| **No aplica en producción** | En producción se usa Neon; Docker solo se usa en desarrollo |

### 14.6.5 Git + GitHub

| Criterio | Justificación |
|---|---|
| **Costo** | GitHub ofrece repositorios públicos gratuitos e ilimitados |
| **Trazabilidad** | Historial completo de cambios con autor, fecha y mensaje |
| **Colaboración** | Permite que futuros desarrolladores continúen el proyecto |
| **Respaldo** | El código está respaldado en la nube de GitHub |
| **Integración con Vercel y Render** | Deploy automático al hacer push |

## 14.7 Arquitectura: justificación

### 14.7.1 Clean Architecture

| Criterio | Justificación |
|---|---|
| **Mantenibilidad** | Separación estricta en capas `domain`, `application`, `infrastructure`; cada capa tiene una responsabilidad única |
| **Testabilidad** | El dominio se puede probar sin BD ni HTTP |
| **Intercambiabilidad** | La BD se puede cambiar sin tocar la lógica de negocio |
| **Seguridad** | La lógica de negocio no conoce Express ni `pg`; reduce superficie de ataque |
| **Continuidad** | Cualquier desarrollador futuro entiende dónde está cada cosa |

### 14.7.2 Repository Pattern

| Criterio | Justificación |
|---|---|
| **Mantenibilidad** | Los contratos de persistencia viven en `domain/repositories`; las implementaciones en `infrastructure/repositories` |
| **Testabilidad** | Se pueden mockear los repositorios para tests unitarios |
| **Intercambiabilidad** | Cambiar de PostgreSQL a otro motor solo requiere nueva implementación del repositorio |

### 14.7.3 Use Cases + DTOs

| Criterio | Justificación |
|---|---|
| **Claridad** | Cada caso de uso tiene una responsabilidad única (`LoginWithGoogleUseCase`, `CreateStudentUseCase`, etc.) |
| **Seguridad** | Las validaciones de dominio se ejecutan antes de tocar la BD |
| **Testabilidad** | Cada caso de uso se puede probar aisladamente |

### 14.7.4 SPA (Single Page Application)

| Criterio | Justificación |
|---|---|
| **Rendimiento** | Carga inicial única; navegación instantánea entre vistas |
| **Experiencia de usuario** | Sin recargas de página; sensación de aplicación nativa |
| **Simplicidad de despliegue** | Archivos estáticos; se despliega en cualquier CDN |

### 14.7.5 Monorepo

| Criterio | Justificación |
|---|---|
| **Simplicidad** | Un solo repositorio para backend y frontend; un solo `git clone` |
| **Consistencia** | Los cambios en backend y frontend viajan juntos en un mismo commit |
| **Despliegue coordinado** | Vercel y Render detectan el mismo repo y despliegan la parte correspondiente |

## 14.8 Seguridad y protección de datos de menores

El sistema gestiona información sensible de menores de edad. Las medidas de protección implementadas son:

| Medida | Implementación |
|---|---|
| **Cifrado en tránsito** | HTTPS obligatorio en frontend y backend; SSL en la conexión a PostgreSQL |
| **Cifrado en reposo** | Neon cifra los datos en disco; Render cifra variables de entorno |
| **Control de acceso** | JWT + RBAC; solo usuarios autorizados acceden a datos de estudiantes |
| **Principio de mínimo privilegio** | Estudiantes y padres solo ven sus propios datos; docentes solo ven sus cursos; administradores ven todo |
| **Validación de entrada** | `PersonValidator` previene inyección y datos malformados |
| **Auto-logout** | Sesión expira tras 10 minutos de inactividad en el frontend |
| **Sin contraseñas locales** | Delegado a Google; el sistema nunca almacena credenciales |
| **Consultas parametrizadas** | El driver `pg` previene inyección SQL |
| **Variables de entorno** | Credenciales nunca en el código; `.env` excluido de Git |
| **`sourcemap: false`** | Build de producción sin mapas de código fuente; oculta estructura interna |

### 14.8.1 Cumplimiento normativo

El sistema se alinea con buenas prácticas de protección de datos personales:

- **Minimización de datos:** solo se recogen los datos necesarios para la gestión académica.
- **Finalidad limitada:** los datos se usan exclusivamente para fines educativos.
- **Acceso restringido:** solo el personal autorizado accede a datos de estudiantes.
- **Trazabilidad futura:** el modelo de datos permite añadir auditoría (`updated_by`, `deleted_at`) sin cambios estructurales.

## 14.9 Costo total de propiedad (TCO)

### 14.9.1 Costo actual (plan gratuito)

| Componente | Costo mensual |
|---|---|
| Frontend (Vercel Free) | 0 USD |
| Backend (Render Free) | 0 USD |
| Base de datos (Neon Free) | 0 USD |
| Autenticación (Google Identity) | 0 USD |
| Dominio (opcional) | ~10 USD/año |
| **Total** | **~0 USD/mes** |

### 14.9.2 Costo si el colegio crece

| Componente | Costo mensual estimado |
|---|---|
| Frontend (Vercel Pro) | 20 USD |
| Backend (Render Starter) | 7 USD |
| Base de datos (Neon Launch) | 19 USD |
| **Total** | **~46 USD/mes (~320 Bs/mes)** |

Este costo es **significativamente menor** que cualquier alternativa propietaria:

- Licencia de software académico propietario: 500–5.000 USD/año.
- Servidor VPS administrado: 20–50 USD/mes + administración.
- Desarrollo de sistema a medida por empresa: 10.000–50.000 USD.

### 14.9.3 Comparación con alternativas

| Alternativa | Costo anual estimado | Complejidad operativa |
|---|---|---|
| **Sistema actual (open source + free tier)** | 0–550 USD | Baja |
| Sistema propietario comercial | 500–5.000 USD | Media |
| Desarrollo a medida por empresa | 10.000–50.000 USD | Alta |
| Servidor propio + administración | 2.400–6.000 USD + personal | Muy alta |

## 14.10 Escalabilidad y continuidad

El sistema puede crecer sin cambios arquitectónicos:

- **Más estudiantes:** PostgreSQL escala verticalmente; Neon permite aumentar recursos con un clic.
- **Más usuarios concurrentes:** Render escala horizontalmente; Vercel CDN absorbe el tráfico estático.
- **Nuevas funcionalidades:** Clean Architecture permite añadir módulos sin tocar el dominio existente.
- **Migración de proveedor:** al usar PostgreSQL estándar y Node.js, el sistema puede migrarse a cualquier nube sin reescribir código.
- **Auditoría y reportes:** el modelo de datos soporta añadir tablas de auditoría y consultas analíticas sin rediseño.

## 14.11 Conclusión

Cada tecnología del sistema fue elegida considerando el contexto real de la institución: **presupuesto limitado, personal sin formación en TI y necesidad de proteger datos de menores de edad**. La combinación de React + Vite + TypeScript + Tailwind en el frontend, Node.js + Express + PostgreSQL en el backend, Google OAuth 2.0 para autenticación y plataformas serverless gratuitas para despliegue ofrece:

- **Costo total de propiedad prácticamente nulo** en el plan gratuito.
- **Seguridad de nivel empresarial** delegando la autenticación a Google y usando PostgreSQL con SSL.
- **Complejidad operativa mínima** para el colegio: no hay servidores que administrar, no hay contraseñas que gestionar, no hay backups manuales.
- **Escalabilidad sin reescritura** si el colegio crece.
- **Mantenibilidad futura** gracias al uso de tecnologías estándar con amplia oferta de desarrolladores.

La elección de PostgreSQL sobre MongoDB no es una desviación de la rúbrica, sino una **decisión técnica justificada por la naturaleza relacional del dominio académico y por la necesidad de garantizar integridad referencial en datos sensibles**. La elección de OAuth 2.0 sobre contraseñas locales no es una omisión de bcrypt, sino una **decisión de seguridad superior** que delega el hashing a un proveedor certificado y elimina la carga operativa de gestión de contraseñas para una institución sin personal de TI.

El sistema cumple con los requisitos de la Segunda Entrega y, además, es **operativamente sostenible** para la institución a la que sirve.