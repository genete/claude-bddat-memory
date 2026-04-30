---
name: Ficheros temp con nombre único — no leer si ya existe
description: Al escribir un fichero temporal nuevo, si el nombre ya existe NO leerlo; crear uno con nombre distinto para evitar gastar tokens en contenido irrelevante
type: feedback
originSessionId: c623581d-83e5-4d0b-9206-02df4707065e
---
Cuando se va a escribir un fichero en `docs_prueba/temp/` (body de issue, body de PR, borrador, etc.) y el nombre destino ya existe, **NO leerlo**. Crear directamente un fichero con nombre alternativo:

- Añadir sufijo con el número de issue/PR: `issue_347_body.md` → `issue_347_nueva.md`
- O sufijo `-v2`, `-draft`, timestamp, lo que sea único

**Why:** Los ficheros temp se acumulan de sesiones anteriores con contenido no relacionado. Leerlos antes de sobreescribir gasta tokens y contexto sin ningún beneficio — el contenido anterior es irrelevante y se va a sobreescribir de todas formas.

**How to apply:** Siempre que el target de `Write` sea un fichero en `temp/` y pueda existir ya, usar directamente un nombre nuevo sin intentar leer el existente. Aplica también al skill `/pr` y similares — ver commit e6aa20e que cambió `gh_body.md` fijo a `pr_body_XX.md` con número de PR por el mismo motivo.
