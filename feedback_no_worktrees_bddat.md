---
name: feedback-no-worktrees-bddat
description: "Carlos probó worktrees (Claude Desktop) en #776 y decidió no usarlos en BDDAT — no ofrecerlos ni sugerirlos de nuevo salvo que él lo pida explícitamente"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fd717512-f9d6-440c-b9b1-63e5b2490b84
  modified: 2026-08-22T08:55:38.420Z
---

Tras probar un worktree real (`.claude/worktrees/issue-776-analysis-8ada05`,
ver [[feedback-worktree-venv-env]] y [[feedback-worktree-settings-local-no-sync]]),
Carlos concluyó (2026-08-22) que no compensa para su forma de trabajar en
BDDAT y decidió no usarlos.

**Por qué:** la fricción de arranque por worktree (reconstruir el bundle
React + `node_modules` de `react-src`, sumada a la sensación de tener que
resolver permisos y rutas cada vez) pesó más que el beneficio de trabajar en
paralelo. Nota honesta para no repetir el error: de los tres problemas vistos
ese día, solo el build de React era un coste real de worktree en sí (git
worktree solo trae ficheros trackeados, venv/node_modules/build son
gitignored a propósito); el venv resultó no estar roto (hipótesis mía
descartada) y los permisos repetidos eran por patrones sin wildcard, no por
el worktree. Aun así, la decisión de Carlos es no usarlos — el diagnóstico
correcto no cambia la conclusión, es su preferencia de flujo de trabajo.

**Cómo aplicar:** no proponer worktrees como solución a "quiero trabajar en
paralelo en varios issues" en este proyecto. Si Carlos lo pide él mismo en el
futuro, sí ayudar, pero no ofrecerlo proactivamente.
