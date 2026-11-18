<!--
Sync Impact Report
- Version change: (plantilla sin ratificar) → 1.0.0
- Modified principles: plantilla genérica (5 marcadores) → 8 principios definidos:
  I. La IA sugiere, el personal de salud decide
  II. Solo datos sintéticos
  III. Diseño guiado por el dominio (DDD)
  IV. Primero las pruebas
  V. Seguridad por defecto
  VI. Independencia de historias
  VII. Stack tecnológico fijo
  VIII. Evidencia en cada cambio
- Added sections: Marco regulatorio y de dominio; Flujo de desarrollo y puertas de calidad
- Removed sections: ninguna
- Templates: no modificados (las plantillas leen la constitución en tiempo de ejecución)
- Follow-up TODOs:
  - docs/glosario.md aún no existe; el Principio III lo exige.
  - Los puntos de extensión del Principio VI deben quedar documentados al crear el esqueleto.
-->

# Triage Twin Constitution

Gemelo digital de triage en servicios de urgencias, alineado con la Resolución 5596 de 2015
del Ministerio de Salud y Protección Social de Colombia.

## Core Principles

### I. La IA sugiere, el personal de salud decide (NO NEGOCIABLE)

- El sistema MUST limitarse a proponer una clasificación de triage, alertas o recomendaciones;
  NUNCA asigna ni cambia la clasificación de un paciente sin la acción explícita de un
  profesional autorizado.
- Toda decisión (aceptar, modificar o rechazar una sugerencia) MUST registrarse en el contexto
  de auditoría con el usuario, su rol, la fecha y hora (UTC, ISO 8601), el valor sugerido y el
  valor decidido.
- El registro de auditoría MUST ser de solo escritura para la aplicación: no se editan ni se
  borran entradas.

**Justificación**: la responsabilidad clínica es del personal de salud; la trazabilidad
protege al paciente y al profesional.

### II. Solo datos sintéticos (NO NEGOCIABLE)

- El código, las pruebas, los fixtures, las semillas de base de datos, los logs, la
  documentación y los prompts a modelos de IA MUST contener únicamente datos sintéticos.
- Queda prohibido incluir nombres, documentos de identidad, historias clínicas u otro dato
  personal real, en cumplimiento de la Ley 1581 de 2012.
- Los generadores de datos sintéticos MUST ser deterministas (semilla fija) para que las
  pruebas sean reproducibles.

**Justificación**: el proyecto es académico y de simulación; no existe base legal para tratar
datos reales de pacientes.

### III. Diseño guiado por el dominio (DDD)

- Cada contexto acotado MUST ser un módulo independiente en `backend/app/contexts/`:
  `pacientes`, `signos`, `evaluacion`, `alertas`, `reglas`, `auditoria`, `iam`.
- Un contexto MUST NOT importar los modelos internos de otro; la comunicación se hace mediante
  interfaces públicas (servicios, esquemas o eventos de dominio) declaradas por cada contexto.
- El lenguaje ubicuo MUST mantenerse en `docs/glosario.md`; todo término de dominio nuevo que
  aparezca en código, especificaciones o UI se agrega al glosario en el mismo cambio.

**Justificación**: fronteras claras permiten evolucionar reglas clínicas sin efectos
colaterales y mantienen un vocabulario común entre clínicos y desarrolladores.

### IV. Primero las pruebas (NO NEGOCIABLE)

- Cada criterio de aceptación MUST tener al menos una prueba automatizada cuyo nombre incluya
  el ID de la historia y del criterio: `test_<HISTORIA>_<CRITERIO>_<descripcion>`
  (por ejemplo, `test_HF04_AC2_alerta_por_saturacion_baja`). En frontend se usa el mismo ID en
  el nombre del `describe`/`it`.
- La prueba MUST escribirse y fallar antes de la implementación (rojo → verde → refactor).
- Un criterio sin prueba trazable se considera no implementado.

**Justificación**: la trazabilidad criterio → prueba es la evidencia de que el sistema hace
lo que la especificación clínica exige.

### V. Seguridad por defecto

- Todo endpoint, salvo los de salud del servicio y de autenticación, MUST exigir un JWT válido
  y autorizar por rol: `enfermeria`, `medico`, `admin`. El acceso se niega por defecto.
- Toda entrada externa (cuerpo, parámetros, cabeceras relevantes) MUST validarse con modelos
  Pydantic; no se procesan diccionarios sin validar.
- Ningún secreto (claves, contraseñas, tokens, cadenas de conexión) MUST estar en el
  repositorio; se cargan desde variables de entorno y se documentan en un `.env.example` sin
  valores reales.
- CORS MUST restringirse a una lista explícita de orígenes configurada por entorno; se prohíbe
  `*` en cualquier entorno distinto del desarrollo local.

**Justificación**: un sistema clínico, aun simulado, debe modelar los controles que exigiría
uno real.

### VI. Independencia de historias

- Cada historia de usuario MUST implementarse principalmente agregando archivos nuevos: su
  router, su carpeta de frontend y su migración de Alembic.
- Las historias MUST integrarse mediante los puntos de extensión definidos (registro de
  routers, registro de rutas del frontend, cadena de migraciones) en lugar de editar archivos
  compartidos.
- Si una historia requiere modificar un archivo compartido, el cambio MUST justificarse en su
  plan y limitarse al mínimo necesario.

**Justificación**: permite que varias historias (y varios agentes) trabajen en paralelo con
conflictos mínimos y que cada historia pueda revertirse por separado.

### VII. Stack tecnológico fijo

- Backend: Python 3.12, FastAPI, SQLAlchemy 2, Alembic, PostgreSQL 16, pytest.
- Frontend: React, Vite, TypeScript, TanStack Query, Vitest.
- Agregar, sustituir o retirar una tecnología de esta lista MUST hacerse mediante una
  enmienda a esta constitución. Las librerías auxiliares compatibles con el stack se justifican
  en el plan de la historia.

**Justificación**: un stack estable reduce la variabilidad entre historias y simplifica la
revisión.

### VIII. Evidencia en cada cambio

- Todo commit MUST incluir el trailer `Agent: <nombre>` que identifique al agente o persona
  que lo produjo.
- Ningún cambio se integra a `main` sin pruebas en verde y sin una revisión de seguridad
  registrada.
- La evidencia de ejecución (resultados de pruebas, capturas) se guarda en `docs/evidencias/`
  y las revisiones en `docs/revisiones/`.

**Justificación**: el proyecto debe poder demostrar quién hizo qué, cómo se verificó y que se
revisó su seguridad.

## Marco regulatorio y de dominio

- La clasificación de triage MUST seguir las cinco categorías de la Resolución 5596 de 2015
  (Triage I a Triage V) y sus tiempos de atención de referencia.
- Las reglas clínicas MUST vivir en el contexto `reglas`, ser configurables y versionadas;
  cada sugerencia registra la versión de reglas que la produjo.
- El sistema es un gemelo digital para simulación y formación; MUST NOT presentarse ni usarse
  como dispositivo médico para la atención real de pacientes.

## Flujo de desarrollo y puertas de calidad

1. Especificar (`/speckit-specify`) → aclarar → planificar → tareas → implementar.
2. Cada plan MUST incluir una verificación de cumplimiento de esta constitución.
3. Puertas antes del merge:
   - pytest y Vitest en verde, incluidas las pruebas trazables a cada criterio de aceptación.
   - Revisión de seguridad (secretos, autorización por rol, validación de entrada, CORS,
     ausencia de datos reales) documentada en `docs/revisiones/`.
   - Commits con el trailer `Agent: <nombre>`.
4. Los cambios que incumplan un principio MUST rechazarse o justificarse explícitamente en el
   plan con la alternativa más simple descartada.

## Governance

- Esta constitución prevalece sobre cualquier otra práctica o guía del proyecto.
- Las enmiendas MUST proponerse por escrito, explicar su motivo e impacto, y actualizar la
  versión de este documento.
- Versionado semántico:
  - MAJOR: eliminación o redefinición incompatible de un principio.
  - MINOR: principio o sección nueva, o ampliación sustancial de una existente.
  - PATCH: aclaraciones y correcciones de redacción.
- Toda revisión de código y todo plan MUST verificar el cumplimiento de los principios; las
  desviaciones se documentan y justifican.

**Version**: 1.0.0 | **Ratified**: 2026-11-18 | **Last Amended**: 2026-11-18
