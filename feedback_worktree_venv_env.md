---
name: feedback-worktree-venv-env
description: "Un worktree nuevo de BDDAT no trae pytest instalado en su venv propio ni copia .env — hay que resolver ambos antes de poder ejecutar tests o arrancar el servidor"
metadata:
  type: feedback
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

**Nota aparte (no de worktree):** los smoke tests de este proyecto corren
contra la BD real de desarrollo (fixtures de rol en `conftest.py` usan el
usuario `CLG` real), no una BD de test aislada. Cualquier test que haga alta
(POST crear) deja filas reales en esa BD si no se limpia explícitamente — ver
el fixture `autouse` de limpieza en
`tests/smoke/test_smoke_catalogo_requerimientos.py` como patrón a seguir en
CRUDs futuros. En #593 esto causó una fuga de filas de prueba visibles en el
listado real hasta que se añadió la limpieza.
