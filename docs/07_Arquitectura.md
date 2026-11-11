# 07 — Arquitectura

## 7.1 Visión general

El sistema implementa una arquitectura cliente-servidor desacoplada con Clean Architecture en ambos extremos. El frontend es una SPA estática; el backend es una API REST stateless que emite JWT; la persistencia es relacional.

## 7.2 Clean Architecture (backend)

```
backend/src/
├── domain/                    # Núcleo — sin dependencias externas
│   ├── entities/              # User, Student, Teacher, Subject, Grade, Attendance, TeacherAssignment, Course
│   ├── repositories/          # Interfaces de repositorio
│   └── validators/            # PersonValidator (reglas de negocio)
├── application/               # Casos de uso
│   ├── dtos/
│   └── use-cases/             # LoginWithGoogleUseCase
├── infrastructure/            # Implementaciones concretas
│   ├── database/              # Pool de conexiones PostgreSQL
│   ├── http/
│   │   ├── controllers/       # AuthController, StudentController
│   │   └── routes/            # 9 routers
│   └── repositories/          # Implementaciones de las interfaces
└── middleware/                # requireAuth, requireRole
```

Regla de dependencia: `infrastructure` → `application` → `domain`. El dominio no conoce Express, `pg` ni Google.

## 7.3 Clean Architecture (frontend)

```
frontend/src/
├── domain/models/             # User, Student, Grade
├── application/use-cases/     # Lógica de vista
├── infrastructure/services/   # authService (fetch a la API)
└── presentation/
    ├── components/            # UI reutilizable
    ├── pages/                 # 11 páginas
    ├── hooks/                 # useAutoLogout
    └── styles/                # Tailwind
```

## 7.4 Capas

| Capa | Responsabilidad | Dependencias permitidas |
|---|---|---|
| `domain` | Entidades, contratos, validaciones | Ninguna |
| `application` | Orquestación de casos de uso | `domain` |
| `infrastructure` | Implementaciones concretas (BD, HTTP) | `application`, `domain` |
| `middleware` | Interceptores HTTP (auth, roles) | `infrastructure` |

## 7.5 Comunicación frontend ↔ backend

- El frontend obtiene un `id_token` de Google y lo envía al backend.
- El backend verifica el token, consulta la BD y emite un JWT propio.
- El frontend almacena el JWT y lo envía en cada petición como `Authorization: Bearer <token>`.
- El backend valida el JWT con `requireAuth` antes de procesar la petición.

## 7.6 Ventajas del diseño

- Desacoplamiento: el frontend puede cambiar sin tocar el backend.
- Testabilidad: el dominio se puede probar sin BD ni HTTP.
- Intercambiabilidad: la BD se puede cambiar sin tocar la lógica de negocio.
- Claridad: cada capa tiene una responsabilidad única.