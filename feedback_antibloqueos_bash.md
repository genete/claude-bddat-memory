---
name: feedback_antibloqueos_bash
description: Claude tiende a olvidar verificar los patrones anti-bloqueo Bash antes de escribir comandos, causando interrupciones innecesarias al usuario
type: feedback
originSessionId: 84025c15-bdb8-4179-baf0-d2bb6dad4326
---
Las reglas concretas están en `docs/guias/REGLAS_BASH.md`. El problema no es desconocerlas sino no verificarlas activamente antes de cada comando Bash.

**Regla para subagentes:** Cuando se delega trabajo a un agente (tool `Agent`), el prompt DEBE incluir la instrucción de leer `docs/guias/REGLAS_BASH.md` antes de ejecutar cualquier comando Bash. Los agentes arrancan sin contexto de sesión y no heredan este conocimiento.

**Why:** En sesiones anteriores se han generado comandos con `$()`, newlines, `sed -i` o comillas en flags que dispararon peticiones de aprobación evitables. Además: usar `rm` o `mv` para limpiar temporales — la regla es NO hacer nada, el usuario los borra manualmente.

**How to apply:** Leer `REGLAS_BASH.md` al inicio de cada sesión, ANTES del primer comando Bash. Fallo recurrente: este paso se omite aunque está en la memoria. No hay excepción.

Puntos críticos a recordar siempre:
- Temporales en docs_prueba/temp/: NO hacer nada — ni rm ni mv (ambos bloqueados). Dejar el fichero; el usuario lo borra manualmente.
- Rutas: siempre `/` (Unix), nunca `\` (Windows)
- Sustitución: nunca `$()` ni backticks — separar en llamadas Bash
- Escritura: nunca `sed -i` ni redirección `>` — usar tools Edit/Write
- **`gh issue create` / `gh pr create` con body multilínea o con `#`:** SIEMPRE `Write` body a `docs_prueba/temp/` → `--body-file`. NUNCA `--body "..."` con saltos de línea o `#`.
- **`git commit` con mensaje multilínea:** `Write` mensaje → `commit -F fichero`. NUNCA `git commit -m "$(cat ...)"` ni heredoc inline.
- **Listados de directorio**: nunca `ls` en Bash — usar `Glob` o `Get-ChildItem` (PowerShell).
