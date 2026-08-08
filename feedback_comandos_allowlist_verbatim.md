---
name: feedback_comandos_allowlist_verbatim
description: "Los comandos rutinarios de la allowlist (build React, etc.) hay que escribirlos VERBATIM — añadir `| tail`, `2>&1` o cualquier extra rompe el match y dispara una petición de permiso innecesaria"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: b60efeba-3d77-41ac-808a-82e5d03bf3d6
  modified: 2026-08-07T07:21:45.179Z
---

Las entradas de `permissions.allow` en `.claude/settings.json` hacen match **literal** sobre el
comando completo. Añadir un pipe, una redirección o cualquier token extra convierte un comando
allowlisted en uno que pide permiso.

Caso que se repite — **compilar los bundles React**. Escribir exactamente:

    npm --prefix /d/BDDAT/react-src run build

Nada de `2>&1 | tail -6`, `&& echo ok` ni variantes. Alternativas también allowlisted:
`bash /d/BDDAT/scripts/build_react.sh` y `bash scripts/build_react.sh`. Es un paso automático de
la rutina de trabajo, no una decisión que Carlos deba aprobar cada vez (2026-08-07, #766).

**Cómo aplicar:** antes de "mejorar" un comando rutinario con pipes para acortar la salida,
comprobar si su forma pelada está en la allowlist — la salida larga sale más barata que un
prompt de permiso. Mismo espíritu que [[feedback_ruta_posix_arranque_run_py_allowlist]] (ruta
POSIX para que case el patrón) y [[feedback_matar_proceso_flask_al_cerrar_navegador]] (TaskStop
en vez de taskkill).
