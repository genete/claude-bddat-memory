---
name: feedback-worktree-venv-env
description: "Un worktree nuevo de BDDAT no trae pytest instalado en su venv propio ni copia .env, y tampoco trae el bundle React compilado ni node_modules de react-src — hay que resolver antes de ejecutar tests, arrancar el servidor o ver la isla React"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fd717512-f9d6-440c-b9b1-63e5b2490b84
  modified: 2026-08-22T08:50:20.498Z
---

Cada worktree (`D:\bddat-<N>`) tiene su propio `venv/` generado a partir de
`requirements.txt`, pero ese fichero no incluye `pytest` (solo dependencias de
producción) y `.env` está gitignored, así que no se copia solo al crear el
worktree.

**Síntomas vistos en #593:**
- `venv/Scripts/python.exe -m pytest` → `No module named pytest`.
- Tras instalar pytest, `create_app()` → `RuntimeError: Either
  'SQLALCHEMY_DATABASE_URI' or 'SQLALCHEMY_BINDS' must be set` (falta `.env`).

**Cómo aplicar:** en un worktree nuevo, antes de correr tests o arrancar el
servidor:
1. `venv/Scripts/python.exe -m pip install pytest` (u otras deps de test que
   falten).
2. Copiar `.env` desde el repo principal (`D:\BDDAT\.env`) — ya es el patrón
   seguido en los worktrees hermanos (`bddat-582`, `bddat-591`), así que no es
   una decisión nueva, solo replicar lo ya hecho.

**Tercera pata del mismo patrón (2026-08-22, worktree Claude Desktop
`.claude/worktrees/issue-776-analysis-8ada05`, distinto del mecanismo
`D:\bddat-<N>` de arriba pero mismo problema de fondo):** la isla React
aparecía sin contenido en el navegador. Causa verificada: `app/static/js/react/*`
está gitignored salvo `manifest.json` (única excepción explícita en
`.gitignore`), así que el worktree hereda el manifest (trackeado) pero no los
`.js`/`.css` reales que ese manifest referencia — los `<script>` apuntan a
ficheros que no existen, 404 silencioso, nada visible salvo mirando
consola/red del navegador. Tampoco existe `react-src/node_modules/` en el
worktree, así que ni siquiera se puede reconstruir sin instalar antes.
**Arreglo:** `npm --prefix react-src install` seguido de
`npm --prefix react-src run build`, ejecutados con cwd dentro del propio
worktree (rutas relativas) para que operen sobre su `react-src` y no sobre el
del repo principal — ver [[feedback-comandos-allowlist-verbatim]], el patrón
ya aprobado `npm --prefix /d/BDDAT/react-src run build` usa ruta absoluta al
principal y NO sirve para reconstruir el bundle de un worktree.

**Nota aparte (no de worktree):** los smoke tests de este proyecto corren
contra la BD real de desarrollo (fixtures de rol en `conftest.py` usan el
usuario `CLG` real), no una BD de test aislada. Cualquier test que haga alta
(POST crear) deja filas reales en esa BD si no se limpia explícitamente — ver
el fixture `autouse` de limpieza en
`tests/smoke/test_smoke_catalogo_requerimientos.py` como patrón a seguir en
CRUDs futuros. En #593 esto causó una fuga de filas de prueba visibles en el
listado real hasta que se añadió la limpieza.
