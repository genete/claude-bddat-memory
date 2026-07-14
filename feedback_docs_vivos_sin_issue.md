---
name: feedback_docs_vivos_sin_issue
description: Ediciones a ADRs y documentos de diseño vivos (no código) no requieren issue de GitHub ni rama feature/; commit directo a develop
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 997ee993-5c88-40d9-aecd-38b93e71e03e
---

Al añadir necesidades N078-N081 a `DETALLE_NECESIDADES_BDDAT.md` (sesión
2026-07-09, etiquetado retroactivo de [[project_adr030_coverage_matrix_pattern]]
ADR-031), propuse crear un issue de GitHub para trackear el trabajo. Carlos:
"no veo necesidad. Commit directo es correcto. No era tarea."

**Por qué:** el ciclo de "rama feature/ antes de editar + PR contra develop"
([[feedback_rama_antes_de_empezar]]) es para cambios de código que necesitan
revisión de un cambio de comportamiento. Un ADR o un documento de diseño vivo
(`DETALLE_NECESIDADES_BDDAT.md`, `MATRIZ_COBERTURA_BDDAT.md`,
`CONTEXTO_ACTUAL.md`) es contenido editorial, no una "tarea" en el sentido del
ciclo de trabajo de ADR-031 — se corrige igual que se corregiría una errata,
sin el aparato de issue/rama/PR.

**Cómo aplicar:** ante una edición de un documento de `docs/decisiones/` o
`docs/diseño/` que no toque código de `app/`, no proponer crear issue ni rama
— hacer commit directo a `develop` con el formato de commit habitual (sin
número de issue si no existe uno, como ya hacían los commits previos de
ADR-031 en `git log`). Si la duda es si algo "es tarea", preguntar antes de
asumir que sí lo es.
