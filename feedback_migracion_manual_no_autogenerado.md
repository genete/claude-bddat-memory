---
name: feedback-migracion-manual-no-autogenerado
description: "Las migraciones de BDDAT se escriben a mano desde el principio, nunca con flask db migrate (autogenerado) + revisión posterior"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 5772ee62-b715-411b-abfa-f0e5bbf52ab9
  modified: 2026-08-05T05:44:41.381Z
---

Nunca proponer el flujo "`flask db migrate` (autogenerado) → revisar el diff → ajustar". En BDDAT las migraciones se escriben directamente a mano con `op.create_table`/`op.add_column` etc., calcadas al estilo ya presente en `migrations/versions/`.

**Why:** Carlos corrigió explícitamente el plan de #728 porque proponía autogenerado como paso del flujo. El repo no sigue ese patrón: revisar migraciones existentes (p.ej. `391_organismos_expediente.py`, `392_diagnosticos.py`, `725_creacion_generica_tramite.py`) confirma que se escriben directamente.

**Convención observada** (para replicar al escribir una migración nueva):
- Nombre de fichero y `revision` = `NNN_descripcion_corta` (NNN = número de issue), no el hash hex que genera Alembic por defecto — aunque conviven ambos estilos en el histórico, las migraciones recientes de una tabla nueva usan `NNN_...`.
- `down_revision` = el head actual exacto (verificar con `flask db heads`, ver [[feedback_alembic_heads]]).
- Docstring del módulo: `"""NNN_descripcion\n\nRevision ID: ...\nRevises: ...\nCreate Date: YYYY-MM-DD\n\nIssue #NNN — explicación breve."""`.
- `GRANT SELECT ON public.<tabla> TO claude_desktop` **inline al final de `upgrade()`**, no como migración separada — la migración separada (`449_grant_organismos_expediente.py`) fue un fix a un olvido, no el patrón a seguir.
- `downgrade()` con `op.drop_table`/`op.drop_column` simétrico; el GRANT se revoca solo al hacer drop de la tabla, no hace falta REVOKE explícito salvo que la migración solo toque grants.

**How to apply:** Al planificar o escribir cualquier migración en BDDAT, saltar directamente a redactar el fichero con este formato — no mencionar ni proponer autogenerado en ningún paso intermedio.
