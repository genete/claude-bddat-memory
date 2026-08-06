---
name: feedback_ruta_posix_arranque_run_py_allowlist
description: Arrancar run.py con Bash usando ruta estilo Windows (D:/BDDAT/...) no coincide con el patrón allowlisted y pide confirmación; usar ruta POSIX (/d/BDDAT/...) como en CLAUDE.md
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 594dda76-6b48-40b7-828d-dc55d0361f8a
  modified: 2026-08-06T07:29:34.708Z
---

`.claude/settings.json` allowlista `Bash(/d/BDDAT/venv/Scripts/python.exe:*)` — un patrón de
**prefijo literal** en formato POSIX (Git Bash / MSYS2). Si el comando de arranque se escribe
con letra de unidad Windows, p. ej. `D:/BDDAT/venv/Scripts/python.exe D:/BDDAT/run.py`, el
texto no coincide con el patrón aunque apunte al mismo fichero, y Claude Code pide
confirmación manual — a diferencia de otras veces en que el arranque no la pidió.

**Por qué:** el matching de la allowlist es por prefijo de string, no por resolución de ruta;
`D:/BDDAT/...` y `/d/BDDAT/...` son literales distintos aunque el shell los resuelva igual.

**Cómo aplicar:** para arrancar `run.py` por Bash (verificación en navegador u otro uso
puntual), usar siempre la forma POSIX documentada en `CLAUDE.md` del proyecto:
`venv/Scripts/python.exe run.py` con `cd` a `/d/BDDAT` en una llamada aparte (no combinado en
la misma línea — ver regla de `cd` en `REGLAS_BASH.md`), o `/d/BDDAT/venv/Scripts/python.exe`
con ruta absoluta POSIX si no se puede evitar el path completo. Nunca `D:/BDDAT/...` en un
comando Bash.

Ver también [[feedback_matar_proceso_flask_al_cerrar_navegador]].
