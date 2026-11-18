---
inclusion: always
---
# Estructura y fuentes de verdad

Fuentes de verdad (léelas antes de revisar o cambiar algo):
- Reglas del proyecto: .specify/memory/constitution.md
- Historias de usuario: docs/backlog.md
- Especificaciones de GitHub Spec Kit: specs/<NNN-nombre>/ (spec.md, plan.md, tasks.md, contracts/)

No crees specs propios de Kiro: las especificaciones solo se hacen con Spec Kit desde Claude Code.

Carpetas:
- backend/app/contexts/<contexto>/: pacientes, signos, evaluacion, alertas, reglas, auditoria, iam
- backend/tests/: pruebas del backend (test_<HISTORIA>_<CRITERIO>_...)
- frontend/src/features/<funcionalidad>/: pantallas por funcionalidad
- docs/revisiones/: hallazgos del agente revisor-seguridad (lo escribe Kiro)
- docs/evidencias/: conversaciones exportadas y reportes de pruebas

Papel de Kiro en este proyecto: revisor de seguridad y calidad. Revisa y reporta; no corrige código.
