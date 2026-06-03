---
name: project_login_dos_pasos
description: Login de BDDAT es de dos pasos para usuarios multi-rol (CLG); cómo autenticar al testear con navegador/fetch
metadata: 
  node_type: memory
  type: project
  originSessionId: 7a5fce0d-f245-4c04-b5d5-1928f56c18bd
---

El login (`app/routes/auth.py`, `POST /auth/login`) es de **dos pasos** para usuarios con
2+ roles. `CLG` (contraseña `31416`) tiene 3 roles: SUPERVISOR (rol_id=2), TRAMITADOR (3),
ADMINISTRATIVO (4). **No hay CSRF** en el formulario.

- **Paso 1** — POST con `siglas` + `password`: valida credenciales pero **NO** hace
  `login_user`; re-renderiza el formulario con un `<select name="rol_id">` (status 200,
  misma URL `/auth/login`). Si solo se hace este POST, la sesión NO queda autenticada
  (síntoma: `/expedientes/...` sigue redirigiendo a login).
- **Paso 2** — POST con `rol_id=<N>`: ahora sí `login_user` + redirect (302 → `next`).

Para fijar el destino tras login, añadir `?next=/ruta` en la URL del **paso 1** (se guarda
en `session['next_after_rol']`). Usuarios de 1 solo rol entran en un único POST.

**Receta fetch (preview/MCP), mantiene cookies con `credentials:'same-origin'`:**
1. `POST /auth/login?next=<destino>` body `siglas=CLG&password=31416` → 200 con selector de rol.
2. `POST /auth/login` body `rol_id=2` → con `redirect:'manual'` devuelve `status:0`
   (redirect opaco) = éxito.
3. navegar a `<destino>`.

El click sobre el botón submit del formulario puede no disparar el POST en el preview; más
fiable `form.requestSubmit()` o el `fetch` directo. Complementa el dato de credenciales del
índice (Stack). Ver también [[feedback_antibloqueos_bash]].
