---
name: feedback-ficheros-temp
description: "Ficheros de docs_prueba/temp/ — nunca rm/mv (bloqueado, el usuario los borra manualmente); si el nombre destino ya existe, usar uno nuevo sin leerlo"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 5304e98c-433b-45f0-8f9a-cb5e84944ca6
  modified: 2026-08-29T07:44:06.857Z
---

**No borrar ni mover.** `rm` y `mv` sobre `docs_prueba/temp/` quedan bloqueados sistemáticamente por el sistema de permisos (el hook `reglas_bash_guard.py` los deniega). El usuario los borra manualmente cuando quiere; la carpeta está gitignored, así que no hay coste en dejarlos indefinidamente.

**Nombre único al escribir uno nuevo.** Si el fichero destino en `docs_prueba/temp/` ya existe (body de issue, body de PR, borrador...), **no leerlo** — crear uno con nombre distinto (sufijo de issue/PR, `-v2`, timestamp...). Los temp se acumulan de sesiones anteriores con contenido no relacionado; leerlos antes de sobreescribir gasta tokens sin ningún beneficio, porque de todas formas se van a sobreescribir. Aplica también a `/pr` y similares (ver commit `e6aa20e`, que cambió `gh_body.md` fijo a `pr_body_XX.md` con número de PR por este mismo motivo).

**How to apply:** tras escribir y usar un temporal, no hacer nada más con él. Al escribir uno nuevo, si el nombre destino puede existir ya, usar directamente un nombre alternativo sin leer el existente.
