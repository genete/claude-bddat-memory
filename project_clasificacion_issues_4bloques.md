---
name: project-clasificacion-issues-4bloques
description: Esquema de clasificación de issues acordado (4 bloques ortogonales multi-etiqueta) y su conexión metodológica con ADR-030
metadata: 
  node_type: memory
  type: project
  originSessionId: 1d33670c-ad89-449a-913a-badc447875c7
---

Carlos propuso (2026-07-07) sustituir una clasificación de issues por zona de código (partición, una zona por issue) por un sistema de **facetas multi-etiqueta ortogonales a los milestones**, donde un issue puede pertenecer a varios bloques a la vez:

- **Bloque 1 — Rol:** ¿útil a supervisor / tramitador / administrativo / admin? (multi-valor)
- **Bloque 2 — Tramitación directa:** ¿avanza la respuesta al ciudadano, o es infra/bug/decoración?
- **Bloque 3 — Mantenimiento del sistema:** core motor, CRUDs estructurales, legacy, mensajería, ayuda, gestión de usuarios.
- **Bloque 4 — Residual:** no encaja en los anteriores (se espera que quede casi vacío).

**Por qué:** los milestones (M1-M5) ya son un eje válido de "cuán imprescindible" (confirmado: M2 "no aguanta más de semanas", M5 "puede añadirse sin comprometer la misión"). Necesitaba un segundo eje que respondiera "para qué sirve" y que permitiera promoción por solapamiento (issue en bloque 2+3 → se prioriza), moderado por dependencias entre issues. Explícitamente limitado a 4 bloques — más le produce "visión borrosa".

**Cómo aplicar:** la clasificación por zona/blueprint de código (ver sesión previa, ~11 zonas: motor-reglas, arbol-expediente, catalogo-documentos, escritos, etc.) NO se descarta — pasa a ser un eje técnico-transversal secundario, útil para que Claude razone dependencias y tipo de trabajo, no para que Carlos decida foco. Los 4 bloques son la clasificación primaria orientada a las personas/decisiones.

**Conexión con [[project_adr030_coverage_matrix_pattern]]:** el mismo principio de "matriz de cobertura con placeholders explícitos" (ADR-030 §9, aplicado a motor/plazos/variables) es el método que falta aplicar a "qué necesita cada rol" y "qué parte del motor está incompleta" — preguntas que ninguna clasificación de issues existentes puede responder, porque los huecos sin issue filed son invisibles a cualquier etiquetado. Es trabajo de auditoría futuro, no de esta sesión.

Ver también [[project_diseno_tarea_analizar_442]] para el contexto de por qué surgió esta necesidad de mapa/clasificación (M2-M3 sensación de "final del túnel borroso").
