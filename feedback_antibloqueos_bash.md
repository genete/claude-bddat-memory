---
name: feedback_antibloqueos_bash
description: Las reglas de REGLAS_BASH.md ya no dependen de recordarlas — las aplica un hook PreToolUse; qué hacer cuando deniega y qué queda fuera de su cobertura
type: feedback
originSessionId: 84025c15-bdb8-4179-baf0-d2bb6dad4326
modified: 2026-07-29T06:16:38.538Z
---

**Resuelto por mecanismo (2026-07-29).** Esta memoria registraba un fallo recurrente:
Claude olvidaba verificar los anti-patrones Bash aunque `CLAUDE.md` lo ordenara y aunque
hubiera leído `docs/guias/REGLAS_BASH.md` al inicio de sesión (recaída confirmada muchos
turnos después, en un comando "trivial"). Carlos zanjó que la solución no era otra memoria.

Ahora lo aplica `.claude/hooks/reglas_bash_guard.py` (hook PreToolUse sobre Bash, declarado
en `.claude/settings.json`): deniega el comando antes de que llegue al usuario, con la regla
y el arreglo concreto en el mensaje.

**How to apply:** si un comando sale denegado con «REGLAS_BASH.md — anti-patrón detectado»,
reescribirlo con el arreglo que indica el mensaje. **Nunca reintentar el mismo comando** —
el guard es determinista, volverá a denegar (ver [[feedback_no_reintentar_latencia]]).

**Qué NO cubre el guard** (excluido a propósito para no generar falsos positivos, que
cuestan tanto como el bloqueo que evitan) — aquí sigue haciendo falta criterio:
- Redirección `>` a fichero: en esta máquina no dispara aprobación y `2>/dev/null` es
  constante. Aun así, para *generar contenido* la vía correcta es la tool `Write`.
- `ls`: preferir `Glob` o `Get-ChildItem`, pero hay entradas de `ls` en la allowlist.
- Temporales en `docs_prueba/temp/`: el guard sí bloquea `rm`/`mv` sobre esa ruta. Si el
  fichero destino ya existe, crear otro con sufijo distinto — no leerlo
  (ver [[feedback_temp_nombre_unico]], [[feedback_rm_temp]]).

Si la tabla de `REGLAS_BASH.md` cambia, actualizar la lista `REGLAS` del guard: el script es
derivado, la guía es la fuente de verdad.
