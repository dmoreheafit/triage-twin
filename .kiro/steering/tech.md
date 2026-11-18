---
inclusion: always
---
# Tecnología

Stack fijo definido en .specify/memory/constitution.md:

- Backend: Python 3.12, FastAPI, SQLAlchemy 2, Alembic, PostgreSQL 16, pytest.
- Frontend: React, Vite, TypeScript, TanStack Query, Vitest.
- Entorno local: docker compose (base de datos, backend y frontend).
- Seguridad: JWT con roles enfermeria, medico y admin; validación con Pydantic; secretos solo en variables de entorno; CORS restringido.
- Herramientas de revisión: bandit y pip-audit (backend), npm audit (frontend).
