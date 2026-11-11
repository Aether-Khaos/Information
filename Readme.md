# Sistema Académico

Sistema de gestión académica para colegio boliviano. Permite administrar estudiantes, docentes, materias, asignaciones, notas y asistencia, con paneles diferenciados por rol (administrador, docente, estudiante y padre/tutor).

**Demo en línea:** https://sistema-academico-eight.vercel.app/

**Repositorio:** [(https://github.com/Aether-Khaos/sistema-academico.git)]

## ¿Qué hace el sistema?

- **Autenticación:** inicio de sesión con Google OAuth 2.0. Solo correos autorizados pueden ingresar.
- **Estudiantes:** registro con validaciones de CI, RUDE, teléfono boliviano, edades por curso y detección de duplicados.
- **Docentes:** alta, edición y control de especialidad y estado.
- **Materias:** CRUD por nivel educativo (inicial, primaria, secundaria).
- **Asignaciones:** vínculo docente–materia–curso.
- **Notas:** registro trimestral por componentes (SER, SABER, HACER, DECIDIR, Autoevaluación).
- **Asistencia:** registro diario por estado (presente, tarde, falta).
- **Paneles por rol:** dashboard distinto para administrador, docente, estudiante y padre/tutor.

## Stack

| Capa | Tecnología |
|---|---|
| Frontend | React 19, Vite 8, TypeScript, Tailwind CSS v4, @react-oauth/google |
| Backend | Node.js 20+, Express 4, TypeScript, pg, jsonwebtoken, google-auth-library |
| Base de datos | PostgreSQL 15+ |
| Autenticación | Google OAuth 2.0 + JWT |

## Cómo levantar el proyecto localmente

### Requisitos

- Node.js 20+
- npm 10+
- Docker + Docker Compose (opcional, para PostgreSQL local)
- Cuenta de Google Cloud con OAuth Client ID

### 1. Clonar el repositorio

```bash
git clone [(https://github.com/Aether-Khaos/sistema-academico.git)]
cd sistema-academico
```

### 2. Configurar variables de entorno

**Backend** (`backend/.env`):

```env
PORT=4000
NODE_ENV=development

# Base de datos
DATABASE_URL="postgresql://USER:PASSWORD@HOST:5432/DBNAME?sslmode=require"

# Google OAuth
GOOGLE_CLIENT_ID=<tu-client-id>.apps.googleusercontent.com

# JWT
JWT_SECRET=<cadena-aleatoria-de-32+-caracteres>
JWT_EXPIRES_IN=7d
```

**Frontend** (`frontend/.env`):

```env
VITE_API_URL=http://localhost:4000/api
VITE_GOOGLE_CLIENT_ID=<tu-client-id>.apps.googleusercontent.com
```

### 3. Levantar PostgreSQL local (opcional)

```bash
docker compose up -d
```

### 4. Instalar dependencias y arrancar

**Backend:**

```bash
cd backend
npm install
npm run dev

```

**Frontend (en otra terminal):**

```bash
cd frontend
npm install
npm run dev

```

## Documentación

La documentación técnica completa está en [`docs/00_Indice.md`](./docs/00_Indice.md), incluyendo arquitectura, modelo de datos, requerimientos, manuales de usuario y administrador.

## Licencia

Uso académico — Proyecto Integrador, asignatura Tecnologías en Internet.%  