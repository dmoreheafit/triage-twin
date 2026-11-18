# Backlog

## Historias funcionales

| ID | Historia | Criterios de aceptación | Contexto | Fase |
|----|----------|-------------------------|----------|------|
| HF-01 | Como enfermera de triage, quiero registrar un paciente clasificado (ID sintético, nivel I–V, hora de triage, población especial) para que el gemelo empiece a vigilarlo | AC1 nivel fuera de I–V responde 422; AC2 aparece en el tablero con espera 0; AC3 solo enfermeria o admin registran | Pacientes | Línea base |
| HF-02 | Como sensor, quiero enviar FC, FR, SpO2, PA sistólica, temperatura y nivel de conciencia para replicar el estado del paciente | AC1 fuera de rango fisiológico responde 422; AC2 guarda la hora original del sensor; AC3 simulador cada N segundos configurable | Signos | Línea base |
| HF-03 | Como enfermera, quiero ver el tablero de espera ordenado por nivel y tiempo de espera para saber a quién atender primero | AC1 orden por nivel y luego mayor espera; AC2 se actualiza cada 10 s o menos; AC3 muestra últimos signos y hora | Tablero | Línea base |
| HF-04 | Como enfermera, quiero un semáforo NEWS2 para adultos y adultos mayores para detectar el deterioro | AC1 0–4 verde, 5–6 o un parámetro en 3 amarillo, 7+ rojo; AC2 muestra puntaje y signos causantes; AC3 casos de prueba de la escala correctos | Evaluación | Línea base |
| HF-05 | Como enfermera, quiero marcar a un paciente como llamado a atención para que salga del tablero | AC1 sale y guarda la hora; AC2 deja de evaluarse; AC3 queda en auditoría | Pacientes | Línea base |
| HF-06 | Como médico, quiero una alerta crítica cuando el nivel sugerido sea I o II y confirmar o descartar la reclasificación | AC1 muestra nivel actual, sugerido y signos; AC2 confirmar cambia el nivel y descartar exige motivo; AC3 decisión registrada con usuario y hora | Alertas | En clase |
| HF-07 | Como administrador, quiero parametrizar los tiempos máximos de espera de los niveles III, IV y V | AC1 I y II no editables; AC2 valores menores o iguales a 0 se rechazan; AC3 cambios auditados | Reglas | En clase |
| HF-08 | Como enfermera, quiero que niños y gestantes se evalúen con escalas pediátrica y obstétrica | AC1 cada población usa su evaluador; AC2 adultos siguen con NEWS2; AC3 el tablero indica la escala aplicada | Evaluación | En clase |
| HF-09 | Como enfermera, quiero ver "sin monitoreo" si un paciente lleva más de 10 min sin lecturas | AC1 indicador gris tras 10 min; AC2 vuelve a normal con una lectura nueva; AC3 umbral configurable | Signos | En clase |
| HF-10 | Como coordinador, quiero ver la línea de tiempo del paciente (lecturas, semáforos, alertas y decisiones) | AC1 orden cronológico; AC2 solo lectura para medico y admin; AC3 muestra quién decidió cada reclasificación | Trazabilidad | En clase |

## Historias técnicas

| ID | Historia | Criterios de aceptación | Contexto | Fase |
|----|----------|-------------------------|----------|------|
| HT-01 | Esqueleto con backend, frontend y PostgreSQL que arranca con docker compose up | AC1 un comando levanta los 3 servicios; AC2 /health responde 200; AC3 puntos de extensión (routers, evaluadores, rutas) | Plataforma | Línea base |
| HT-02 | Modelo de datos con migraciones Alembic y semilla sintética | AC1 alembic upgrade head desde cero; AC2 15 pacientes sintéticos y 3 usuarios; AC3 ningún dato real | Datos | Línea base |
| HT-03 | Autenticación JWT con roles enfermeria, medico y admin | AC1 sin token 401 y sin permiso 403; AC2 contraseñas con argon2; AC3 token expira a los 60 min | Seguridad | Línea base |
| HT-04 | Bitácora de auditoría no editable | AC1 usuario, acción, entidad y hora; AC2 sin endpoint de edición ni borrado; AC3 sin datos sensibles innecesarios | Seguridad | Línea base |
| HT-05 | Pipeline de CI con lint, pruebas y cobertura | AC1 ruff y eslint; AC2 pytest y vitest con cobertura de 70 % o más; AC3 el PR no se une si falla | DevOps | Pendiente |
| HT-06 | Escaneos de seguridad en CI | AC1 bandit, pip-audit, npm audit y gitleaks; AC2 CodeQL y Trivy; AC3 lo crítico bloquea y se guardan reportes | DevSecOps | Pendiente |
| HT-07 | Despliegue repetible en Azure | AC1 imagen en GHCR corriendo en Container Apps; AC2 frontend en Static Web Apps con HTTPS; AC3 secretos solo en variables de entorno | DevOps | Pendiente |
| HT-08 | Regresión clínica con escenarios de deterioro conocidos | AC1 al menos 5 escenarios; AC2 cero falsos negativos en críticos; AC3 corre con pytest -m clinico | Calidad | Pendiente |
| HT-09 | Endurecer la API | AC1 límite por minuto responde 429; AC2 cabeceras HSTS, X-Content-Type-Options y CSP; AC3 CORS solo para el frontend | Seguridad | Pendiente |
| HT-10 | Observabilidad básica | AC1 /ready falla sin base de datos; AC2 logs JSON con request_id; AC3 ningún log con datos del paciente salvo su ID | Operación | Pendiente |
