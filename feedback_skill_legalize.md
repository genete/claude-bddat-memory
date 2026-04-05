---
name: feedback_skill_legalize
description: El skill /legalize solo se invoca por orden directa del usuario, nunca automáticamente desde /boe o /boja
type: feedback
---

El skill `/legalize` es standalone: solo se usa cuando el usuario lo pide explícitamente.
Los skills `/boe` y `/boja` NO llaman a `/legalize` internamente.

**Why:** Cuando `/boja` llamaba a `/legalize` y luego a `/boe`, y `/boe` también llamaba a `/legalize`,
se producían dos llamadas extra innecesarias a un resultado que ya se conocía (NOT_FOUND).
Detectado en prueba con Ley 2/2026 el 2026-04-05.

**How to apply:** Si necesitas consultar si una norma está en legalize-es, usa `/legalize` tú mismo.
Si necesitas leer legislación andaluza, usa `/boja` directamente (que intentará BOE para Leyes,
sedeboja para el resto). El orden de consulta lo gestiona el usuario, no los skills entre sí.
