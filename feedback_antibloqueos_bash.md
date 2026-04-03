---
name: feedback_antibloqueos_bash
description: Claude tiende a olvidar verificar los patrones anti-bloqueo Bash antes de escribir comandos, causando interrupciones innecesarias al usuario
type: feedback
---

Las reglas concretas están en `docs/REGLAS_BASH.md`. El problema no es desconocerlas sino no verificarlas activamente antes de cada comando Bash.

**Why:** En sesiones anteriores se han generado comandos con `$()`, newlines, `sed -i` o comillas en flags que dispararon peticiones de aprobación evitables. Además: usar `rm` en vez de `mv` para limpiar temporales — la regla es mover a `docs_prueba/temp/trash/`, no borrar.

**How to apply:** Antes de escribir cualquier comando Bash, pasada mental contra `REGLAS_BASH.md`. Puntos críticos a recordar siempre:
- Temporales: `mv fichero docs_prueba/temp/trash/` — NUNCA `rm`
- Rutas: siempre `/` (Unix), nunca `\` (Windows)
- Sustitución: nunca `$()` ni backticks — separar en llamadas Bash
- Escritura: nunca `sed -i` ni redirección `>` — usar tools Edit/Write
