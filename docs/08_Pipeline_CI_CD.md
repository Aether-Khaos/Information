# 08 — Pipeline CI/CD

## 8.1 Estado actual

El proyecto no cuenta con un pipeline CI/CD automatizado. El despliegue se realiza de forma manual.

| Componente | Método de despliegue |
|---|---|
| Frontend | Build automático al hacer push a `main` |
| Backend | Build automático al hacer push a `main` |
| Base de datos | Creación manual de tablas |

## 8.2 Integración continua (propuesta)

### 8.2.1 Lint y type-check

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: cd backend && npm ci
      - run: cd backend && npx tsc --noEmit
      - run: cd backend && npm run build

  frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: cd frontend && npm ci
      - run: cd frontend && npx oxlint
      - run: cd frontend && npm run build
```

### 8.2.2 Tests (pendiente de implementar)

```yaml
      - run: cd backend && npm test
      - run: cd frontend && npm test
```

## 8.3 Despliegue continuo (propuesta)

| Entorno | Trigger | Componente |
|---|---|---|
| Preview | Pull request | Frontend |
| Staging | Push a `develop` | Backend |
| Producción | Push a `main` | Ambos |

## 8.4 Roadmap CI/CD

| Prioridad | Tarea | Esfuerzo |
|---|---|---|
| Alta | Workflow de lint + type-check en GitHub Actions | 1 h |
| Media | Tests unitarios del dominio (`PersonValidator`) | 2 h |
| Media | Tests de integración de endpoints críticos | 3 h |
| Baja | Entornos staging y producción separados | 2 h |
| Baja | Análisis de seguridad (`npm audit`, Snyk) | 1 h |