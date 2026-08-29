---
name: feedback-no-worktrees-bddat
description: "Carlos probó worktrees (Claude Desktop) en #776 y decidió no usarlos en BDDAT — no ofrecerlos ni sugerirlos de nuevo salvo que él lo pida explícitamente; postmortem técnico de qué falló"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fd717512-f9d6-440c-b9b1-63e5b2490b84
  modified: 2026-08-29T07:44:50.067Z
---

Tras probar un worktree real (`.claude/worktrees/issue-776-analysis-8ada05`), Carlos concluyó
(2026-08-22) que no compensa para su forma de trabajar en BDDAT y decidió no usarlos.

**Por qué:** la fricción de arranque por worktree (reconstruir el bundle React + `node_modules`
de `react-src`, sumada a la sensación de tener que resolver permisos y rutas cada vez) pesó más
que el beneficio de trabajar en paralelo. Nota honesta para no repetir el error: de los tres
problemas vistos ese día, solo el build de React era un coste real de worktree en sí (git
worktree solo trae ficheros trackeados; venv/node_modules/build son gitignored a propósito) —
el venv resultó no estar roto (hipótesis descartada, ver postmortem) y los permisos repetidos
eran por patrones sin wildcard, no por el worktree. Aun así, la decisión de Carlos es no
usarlos — el diagnóstico correcto no cambia la conclusión, es su preferencia de flujo.

**Cómo aplicar:** no proponer worktrees como solución a "quiero trabajar en paralelo en varios
issues" en este proyecto. Si Carlos lo pide él mismo en el futuro, sí ayudar — con el
postmortem de abajo como checklist de qué resolver antes de dar por listo el worktree.

## Postmortem técnico (por si se retoma en el futuro)

**Mecanismo `D:\bddat-<N>` (worktrees manuales):** `venv/` se genera desde `requirements.txt`,
que no incluye `pytest` (solo deps de producción) → `No module named pytest`. `.env` está
gitignored, no se copia solo → `RuntimeError: SQLALCHEMY_DATABASE_URI must be set`. Antes de
correr tests o arrancar el servidor: `venv/Scripts/python.exe -m pip install pytest` +
copiar `.env` desde `D:\BDDAT\.env` (patrón ya seguido en `bddat-582`, `bddat-591`).

**Mecanismo `.claude/worktrees/<nombre>/` (Claude Desktop):** problema distinto, mismo origen
gitignore. `app/static/js/react/*` está gitignored salvo `manifest.json` (única excepción
explícita) — el worktree hereda el manifest pero no los `.js`/`.css` reales que referencia:
404 silencioso, isla React vacía sin nada en consola salvo mirando red. Tampoco existe
`react-src/node_modules/`. Arreglo: `npm --prefix react-src install` + `run build`, con cwd
dentro del propio worktree (rutas relativas) — el patrón allowlisted
`npm --prefix /d/BDDAT/react-src run build` (ver [[feedback_comandos_allowlist_verbatim]])
usa ruta absoluta al principal y NO sirve para reconstruir el bundle de un worktree.

**`settings.local.json` no sincroniza entre worktree y principal (confirmado, Windows):** la
app siembra una copia puntual al crear el worktree (heredó 41 entradas ya aprobadas), pero no
hay sincronización posterior en ningún sentido. Confirmado contra doc oficial: en Windows la
mejora que centraliza aprobaciones en el checkout principal (v2.1.211) está explícitamente
excluida. **Pero esto normalmente NO es la causa de preguntas de permiso repetidas** — se
diagnosticó mal una vez (se asumió venv en ruta distinta, descartado). La causa real casi
siempre: los patrones `Bash(...)` de BDDAT son cadenas literales sin wildcard, y trabajar en
un issue nuevo implica ejecutar combinaciones de comandos nunca aprobadas antes (nuevo
fichero de test, subcomando de `flask db` no usado hasta ahora...) — pasaría igual sin
worktree. **Resolución que funcionó:** generalizar patrones exactos a wildcard, p. ej.
`Bash(D:/BDDAT/venv/Scripts/python.exe -m pytest *)`,
`Bash(D:/BDDAT/venv/Scripts/python.exe -m flask db *)`, en vez de aprobar cada combinación.

**Nota aparte (no es de worktree):** los smoke tests de este proyecto corren contra la BD real
de desarrollo (`conftest.py` usa el usuario `CLG` real), no una BD de test aislada. Cualquier
test que haga alta deja filas reales si no se limpia — ver el fixture `autouse` de
`tests/smoke/test_smoke_catalogo_requerimientos.py` como patrón a seguir en CRUDs futuros.
