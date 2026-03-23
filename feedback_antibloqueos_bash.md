---
name: feedback_antibloqueos_bash
description: Claude tiende a olvidar verificar los patrones anti-bloqueo Bash antes de escribir comandos, causando interrupciones innecesarias al usuario
type: feedback
---

Las reglas concretas están en `docs/fuentesIA/ANTI_BLOQUEOS_BASH.md`. El problema no es desconocerlas sino no verificarlas activamente antes de cada comando Bash.

**Why:** En sesiones anteriores se han generado comandos con `$()`, newlines, `sed -i` o comillas en flags que dispararon peticiones de aprobación evitables, interrumpiendo el flujo de trabajo en sesiones "edits allowed".

**How to apply:** Antes de escribir cualquier comando Bash, hacer una pasada mental contra `ANTI_BLOQUEOS_BASH.md`. No es suficiente con tenerlo documentado — hay que aplicarlo en el momento de escribir el comando.
