---
name: feedback-worktree-settings-local-no-sync
description: settings.local.json no sincroniza entre worktree y repo principal (confirmado); pero las preguntas de permiso repetidas en un worktree normalmente NO son por eso — ver causa real corregida abajo
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fd717512-f9d6-440c-b9b1-63e5b2490b84
  modified: 2026-08-22T08:42:48.211Z
---

Al crear un worktree desde Claude Desktop (checkbox "árbol de trabajo" +
elegir rama de merge), la app siembra `.claude/worktrees/<nombre>/.claude/settings.local.json`
como **copia puntual** del repo principal en el momento de creación (verificado
con `diff`: heredó las 41 entradas ya aprobadas). Pero a partir de ahí **no hay
sincronización posterior en ningún sentido** — ni el worktree recibe nuevas
aprobaciones hechas en el principal, ni el principal recibe las hechas en el
worktree. Confirmado contra la doc oficial (`docs/en/settings.md`,
"Where Claude Code keeps the local file"): en **Windows** el fichero se queda
siempre en el directorio de arranque — la mejora de v2.1.211 que centraliza
las aprobaciones en el checkout principal está explícitamente excluida en
Windows, así que actualizar Claude Code no lo arregla.

**CORRECCIÓN (2026-08-22, mismo hilo):** la primera vez diagnostiqué mal la
causa de las preguntas repetidas — asumí que el worktree tenía su propio
`venv` en otra ruta absoluta (extrapolando de [[feedback-worktree-venv-env]],
que es sobre worktrees creados a mano tipo `D:\bddat-<N>`, un mecanismo
distinto). **Falso para los worktrees de Claude Desktop en
`.claude/worktrees/<nombre>/`**: verificado con `ls` que ese directorio NO
tiene `venv/` propio — todos los comandos usan el venv del principal
(`D:/BDDAT/venv/...`) vía ruta absoluta, idéntica a como se invoca desde el
repo principal. Por eso pytest nunca fallaba con "No module named" en el caso
real que motivó esta memoria.

**Causa real:** los patrones `Bash(...)` de BDDAT son cadenas literales sin
wildcard, aprobadas por combinación EXACTA de argumentos (ver
`feedback_comandos_allowlist_verbatim` — mismo principio). Trabajar en un
issue nuevo implica casi siempre ejecutar ficheros de test nuevos o distintas
combinaciones de ellos, y aplicar comandos que nunca se habían aprobado antes
(p.ej. `flask db upgrade`, aprobado por primera vez en #776 pese a llevar
meses de proyecto). Esto pasaría exactamente igual sin worktree — la
coincidencia temporal (worktree nuevo + issue nuevo) es lo que lo hizo parecer
un problema de worktree.

**Resolución aplicada (2026-08-22):** en vez de patrones exactos por cada
combinación de ficheros, se generalizaron a wildcard en `D:\BDDAT\.claude\settings.local.json`
y en el `settings.local.json` del worktree activo:
```
Bash(D:/BDDAT/venv/Scripts/python.exe -m pytest *)
Bash(D:/BDDAT/venv/Scripts/python.exe -m flask db *)
```
Cubre cualquier combinación de ficheros/flags de pytest y cualquier
subcomando de `flask db` (upgrade, downgrade, current, heads, merge...) sin
volver a preguntar. Las entradas exactas que quedaban como subconjunto se
retiraron del worktree para no acumular clutter.

**Cómo aplicar:** si un worktree nuevo vuelve a generar muchas preguntas de
permiso, comprobar primero si son combinaciones nuevas de comandos ya
conocidos (solución: generalizar el patrón a wildcard) antes de sospechar de
rutas de venv — verificar con `ls <worktree>/venv` si de verdad existe un venv
propio antes de asumirlo. El hardlink real de `settings.local.json` entre
worktrees sigue evaluado y pendiente si algún día se quiere sincronización
automática de aprobaciones (Carlos no lo ha pedido).
