# Triage Twin: instrucciones para Claude Code

Gemelo digital de triage en urgencias. Proyecto académico con Spec-Driven Development (Spec Kit) y arquitectura multiagente.

## Fuentes de verdad
- Reglas del proyecto: .specify/memory/constitution.md
- Historias: docs/backlog.md
- Especificaciones: specs/

## Reglas del agente líder
- No implementes nada sin spec.md, plan.md y tasks.md aprobados por el humano.
- En tasks.md etiqueta cada tarea [QA], [BE] o [FE] y refiérela a su historia y criterio (por ejemplo HF-01 AC2).
- En /speckit-implement NO escribas código tú. Orden: 1) qa-tester escribe las pruebas que fallan; 2) backend-dev y frontend-dev implementan, en paralelo en las tareas [P]; 3) qa-tester verifica.
- Antes de integrar a main lee docs/revisiones/<historia>.md (lo escribe Kiro) y corrige cada hallazgo alto o crítico.
- Nunca uses datos reales de pacientes.

## Comandos útiles
- Levantar todo: docker compose up -d --build
- Pruebas backend: docker compose exec backend pytest -q
- Pruebas frontend: cd frontend && npm test -- --run
