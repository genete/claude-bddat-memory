---
name: feedback_antibloqueos_bash
description: Claude tiende a olvidar verificar los patrones anti-bloqueo Bash antes de escribir comandos, causando interrupciones innecesarias al usuario
type: feedback
originSessionId: 84025c15-bdb8-4179-baf0-d2bb6dad4326
---
Las reglas concretas están en `docs/guias/REGLAS_BASH.md`. El problema no es desconocerlas sino no verificarlas activamente antes de cada comando Bash.

**Why:** En sesiones anteriores se han generado comandos con `$()`, newlines, `sed -i` o comillas en flags que dispararon peticiones de aprobación evitables. Además: usar `rm` o `mv` para limpiar temporales — la regla es NO hacer nada, el usuario los borra manualmente.

**How to apply:** Antes de escribir cualquier comando Bash, pasada mental contra `REGLAS_BASH.md`. Puntos críticos a recordar siempre:
- Temporales en docs_prueba/temp/: NO hacer nada — ni rm ni mv (ambos bloqueados). Dejar el fichero; el usuario lo borra manualmente.
- Rutas: siempre `/` (Unix), nunca `\` (Windows)
- Sustitución: nunca `$()` ni backticks — separar en llamadas Bash
- Escritura: nunca `sed -i` ni redirección `>` — usar tools Edit/Write
