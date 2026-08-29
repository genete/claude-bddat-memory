---
name: feedback_comandos_allowlist_verbatim
description: "Los patrones de permissions.allow hacen match LITERAL — ruta POSIX exacta para run.py, y ni pipes ni redirecciones extra en comandos rutinarios (build React...), o rompen el match y piden permiso"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: b60efeba-3d77-41ac-808a-82e5d03bf3d6
  modified: 2026-08-29T07:44:33.136Z
---

Las entradas de `permissions.allow` en `.claude/settings.json` hacen match **literal** sobre
el comando completo — dos gotchas concretos, mismo principio de fondo.

**1. Ruta POSIX exacta al arrancar `run.py`.** `.claude/settings.json` allowlista
`Bash(/d/BDDAT/venv/Scripts/python.exe:*)` — un patrón de prefijo literal en formato POSIX
(Git Bash/MSYS2). Si el comando se escribe con letra de unidad Windows
(`D:/BDDAT/venv/Scripts/python.exe D:/BDDAT/run.py`), el texto no coincide aunque apunte al
mismo fichero, y Claude Code pide confirmación manual. Usar siempre la forma POSIX
documentada en `CLAUDE.md`: `venv/Scripts/python.exe run.py` con `cd` a `/d/BDDAT` en una
llamada aparte (no combinado en la misma línea), o `/d/BDDAT/venv/Scripts/python.exe` con
ruta absoluta POSIX si no se puede evitar el path completo. Nunca `D:/BDDAT/...` en Bash.

**2. Sin pipes ni redirecciones en comandos rutinarios.** Añadir un pipe, una redirección o
cualquier token extra a un comando ya allowlisted lo convierte en uno que pide permiso. Caso
que se repite — compilar los bundles React: escribir exactamente
`npm --prefix /d/BDDAT/react-src run build`, nada de `2>&1 | tail -6` ni `&& echo ok`.
Alternativas también allowlisted: `bash /d/BDDAT/scripts/build_react.sh` y
`bash scripts/build_react.sh`. Es un paso automático de la rutina de trabajo, no una decisión
que Carlos deba aprobar cada vez (2026-08-07, #766).

**Why:** el matching es por prefijo/patrón de string, no por resolución de ruta ni de
semántica de shell — cualquier desviación literal del patrón exacto dispara una petición de
permiso innecesaria.

**How to apply:** antes de "mejorar" un comando rutinario con pipes para acortar la salida, o
de escribir una ruta con letra de unidad Windows en Bash, comprobar primero la forma pelada
exacta que está en la allowlist — la salida larga o el paso extra de `cd` salen más baratos
que un prompt de permiso. Ver [[feedback_matar_proceso_flask_al_cerrar_navegador]] (TaskStop
en vez de taskkill, mismo espíritu de no salirse de lo ya aprobado).
