---
name: feedback_matar_proceso_flask_al_cerrar_navegador
description: "Al terminar una verificación en navegador que arrancó run.py en background, matar ese proceso además de cerrar el navegador — dejarlo vivo causa acumulación de procesos en el mismo puerto (#630) y comandos de arranque posteriores que fallan"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 594dda76-6b48-40b7-828d-dc55d0361f8a
  modified: 2026-08-06T07:29:23.581Z
---

Cerrar el navegador (`browser_close`) al terminar una verificación NO es suficiente cuando la
verificación arrancó `run.py` con la tool Bash en background (`run_in_background: true`) para
tener un servidor local sobre el que probar. Hay que matar también ese proceso — con
`KillShell`/`TaskStop` sobre el `task-id`, o `taskkill` sobre el PID — antes de cerrar el hilo.

**Por qué:** dejar el proceso vivo es exactamente la causa raíz de
[[feedback_puertos_zombis_windows_run_py]] — Windows permite varios `python run.py`
escuchando el mismo puerto sin error, y arranques posteriores (misma sesión u otra) se
acumulan sobre ese proceso huérfano. En esta sesión (#729, 2026-08-06) un segundo intento de
arrancar `run.py` en background falló con exit code 1 tras cerrar el navegador sin matar el
proceso anterior — síntoma consistente con la acumulación descrita en esa memoria.

**Cómo aplicar:** al terminar cualquier verificación en navegador (Playwright MCP, según
CLAUDE.md) que dependió de un `run.py` propio arrancado por Claude en esta sesión:
1. `browser_close` (regla ya existente en CLAUDE.md del proyecto).
2. Matar el proceso Flask de verificación — no dejarlo "por si acaso" para la próxima vez.

No aplica al servidor que el propio Carlos arranca manualmente (`!` en el prompt) — ese lo
gestiona él.
