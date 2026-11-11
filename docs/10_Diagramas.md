# 10 — Diagramas

## 10.1 Diagrama C4 — Nivel 2 (Contenedores)

```mermaid
C4Container
    title Sistema Académico — C4 Nivel 2

    Person(admin, "Administrador", "Dirección / Secretaría")
    Person(teacher, "Docente", "Registra notas y asistencia")
    Person(student, "Estudiante", "Consulta su panel")
    Person(parent, "Padre/Tutor", "Consulta panel de su hijo")

    System_Boundary(sys, "Sistema Académico") {
        Container(spa, "SPA React", "React + Vite + Tailwind", "Interfaz de usuario")
        Container(api, "API REST", "Node.js + Express + TypeScript", "Endpoints REST")
        ContainerDb(db, "PostgreSQL", "PostgreSQL 15+", "Persistencia relacional")
    }

    System_Ext(google, "Google Identity", "OAuth 2.0 / OpenID Connect")

    Rel(admin, spa, "Usa", "HTTPS")
    Rel(teacher, spa, "Usa", "HTTPS")
    Rel(student, spa, "Usa", "HTTPS")
    Rel(parent, spa, "Usa", "HTTPS")

    Rel(spa, google, "Login OAuth", "HTTPS")
    Rel(spa, api, "Llama endpoints", "JSON/HTTPS + JWT Bearer")
    Rel(api, google, "verifyIdToken", "HTTPS")
    Rel(api, db, "Lee/Escribe", "pg Pool + SSL")
```

## 10.2 Diagrama de secuencia — Autenticación OAuth + JWT

```mermaid
sequenceDiagram
    participant U as Usuario
    participant F as SPA React
    participant G as Google Identity
    participant A as API REST
    participant DB as PostgreSQL

    U->>F: Click "Iniciar sesión con Google"
    F->>G: GoogleLogin component
    U->>G: Ingresa credenciales
    G-->>F: id_token
    F->>A: POST /api/auth/google { idToken }
    A->>G: verifyIdToken({ idToken, audience })
    G-->>A: payload { email, email_verified }
    alt email no verificado
        A-->>F: 401
    end
    A->>DB: SELECT * FROM users WHERE email = $1
    alt usuario no existe
        DB-->>A: null
        A-->>F: 401 "Correo no registrado"
    end
    alt is_active = false
        A-->>F: 401 "Usuario desactivado"
    end
    A->>A: jwt.sign({ userId, email, role }, JWT_SECRET, { expiresIn })
    A-->>F: 200 { token, user }
    F->>F: localStorage.setItem('token', token)
    Note over U,DB: Peticiones posteriores:
    F->>A: GET /api/students (Authorization: Bearer <token>)
    A->>A: requireAuth → jwt.verify
    A-->>F: 200 [...]
```

## 10.3 Diagrama entidad-relación

```mermaid
erDiagram
    USERS ||--o| STUDENTS : "student_id"
    USERS ||--o| TEACHERS : "teacher_id"
    TEACHERS ||--o{ TEACHER_ASSIGNMENTS : "tiene"
    SUBJECTS ||--o{ TEACHER_ASSIGNMENTS : "asignada a"
    STUDENTS ||--o{ GRADES : "recibe"
    SUBJECTS ||--o{ GRADES : "evaluada en"
    TEACHERS ||--o{ GRADES : "registra"
    STUDENTS ||--o{ ATTENDANCE : "asiste"
    SUBJECTS ||--o{ ATTENDANCE : "en"
    TEACHERS ||--o{ ATTENDANCE : "registra"
```

## 10.4 Diagrama de capas (Clean Architecture)

```mermaid
flowchart TD
    A[presentation / infrastructure] --> B[application]
    B --> C[domain]
    style C fill:#f9f,stroke:#333
    style B fill:#ff9,stroke:#333
    style A fill:#9f9,stroke:#333
```

## 10.5 Diagrama de despliegue

```mermaid
flowchart LR
    U[Usuario] -->|HTTPS| F[SPA React]
    F -->|HTTPS JSON| A[API REST]
    A -->|SSL| DB[(PostgreSQL)]
    F -->|OAuth| G[Google Identity]
    A -->|verifyIdToken| G
```