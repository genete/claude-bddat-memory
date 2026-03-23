---
name: feedback_antibloqueos_bash
description: Aplicar activamente los patrones anti-bloqueo de CLAUDE.md antes de escribir cada comando Bash, no solo tenerlos documentados
type: feedback
---

Aunque los anti-bloqueos Bash están documentados en `docs/fuentesIA/ANTI_BLOQUEOS_BASH.md` y referenciados en `CLAUDE.md`, en sesiones anteriores he generado comandos que los violaban, causando interrupciones con peticiones de permiso evitables.

**Why:** Los patrones están documentados pero no se comprueban activamente antes de generar cada comando. El usuario ha tenido que aprobar manualmente comandos con `$()`, newlines, `sed`, o comillas en flags, que deberían haberse evitado desde el inicio.

**How to apply:** Antes de escribir cualquier comando Bash, revisar mentalmente contra la lista:
- ¿Usa `$()`o backticks? → separar en llamadas secuenciales
- ¿Contiene saltos de línea o `#`? → `Write` a `docs_prueba/temp/` + `-F`/`--body-file`
- ¿Usa `sed -i`? → usar tool `Edit`
- ¿Tiene comillas dentro de nombres de flags? → fichero temporal
- ¿Usa `cd` + git? → `git -C /ruta`
