---
name: feedback-alembic-heads
description: Verificar heads de Alembic antes de crear cada migración para evitar ramas divergentes
metadata: 
  node_type: memory
  type: feedback
  originSessionId: ce18fc3c-6a31-4927-811a-2b8e89ca208e
---

Antes de crear cualquier migración nueva, ejecutar `flask db current` y confirmar que hay **un único head**. Si hay múltiples heads, resolverlos primero con un merge point antes de añadir la nueva migración.

**Why:** Las ramas de features que generan migraciones en paralelo sin coordinarse crean heads divergentes. El error se manifiesta al intentar aplicar `flask db upgrade` con "Multiple head revisions are present". En #470 la migración 488 (PR #490) usó `down_revision = '416_seed_plazos_tablon'` sin conectarse con la cadena 456→460, creando dos ramas paralelas que hubo que fusionar manualmente en 470.

**How to apply:** 
- Al inicio de cada sesión que vaya a crear migraciones: `flask db heads` / `flask db current`
- Si hay N heads: crear primero una migración de merge con `down_revision = (head_A, head_B, ...)`
- La migración de merge puede ir vacía (sin lógica upgrade/downgrade) o combinarse con la primera migración real del issue
