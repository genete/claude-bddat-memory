---
name: feedback_matar_proceso_flask_al_cerrar_navegador
description: "Windows permite varios run.py escuchando el mismo puerto sin error; al terminar una verificación en navegador, matar el proceso run.py (TaskStop) además de cerrar el navegador — dejarlo vivo acumula procesos zombis"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 594dda76-6b48-40b7-828d-dc55d0361f8a
  modified: 2026-08-29T07:44:22.798Z
---

Cerrar el navegador (`browser_close`) al terminar una verificación NO es suficiente cuando la
verificación arrancó `run.py` con la tool Bash en background (`run_in_background: true`). Hay
que matar también ese proceso.

**La forma correcta es `TaskStop` sobre el `task_id`** que devolvió la propia llamada Bash —
no pide permiso, es la rutina normal del ciclo servidor→pruebas→`browser_close`→parar servidor.
**No usar `taskkill` ni `Stop-Process`/PowerShell**: no están en la allowlist, disparan una
petición de permiso innecesaria y Carlos ya ha señalado esto como superado (2026-08-07, #684).

**Por qué importa (causa raíz, sesión #630/PR #756, 2026-08-04):** Windows **no** exige
`SO_EXCLUSIVEADDRUSE` para el `bind()` de un socket TCP — a diferencia de Linux, varios
procesos `python run.py` pueden quedar **todos** escuchando en el mismo puerto (p. ej. 5000)
sin que ninguno arranque con error de "puerto ocupado". El SO enruta peticiones de forma
impredecible entre ellos (aparentemente favorece el más antiguo). Cada arranque nuevo se ve
limpio en su propio log — el síntoma es que el navegador sigue viendo el comportamiento del
proceso viejo (p. ej. un blueprint que no existía antes de un cambio de código reciente) por
más que se "reinicie" el servidor. En esa sesión, `Get-CimInstance Win32_Process` reveló 14
procesos acumulados pese a varios `taskkill`/`Stop-Process` previos que reportaban éxito.

Dejar el proceso vivo al terminar una verificación es exactamente la causa de esta
acumulación — de ahí que matarlo con `TaskStop` sea la prevención, no solo una cortesía.

**Si el síntoma reaparece** (un 500/404 persiste de forma idéntica tras "reiniciar" el
servidor, sospechar acumulación antes que bug de código):
1. `Get-CimInstance Win32_Process -Filter "Name='python.exe'" | Where-Object { $_.CommandLine -like '*run.py*' } | Select ProcessId,CreationDate,CommandLine` (PowerShell, vía `mcp__windows-mcp__PowerShell` — más fiable que `netstat`/`taskkill` desde Git Bash, que dio PIDs fantasma inconsistentes).
2. Matar **todos** los procesos encontrados de golpe, no solo "el último".
3. Verificar limpio: `Get-NetTCPConnection -LocalPort <puerto>` sin filas, conteo de `run.py` a cero.

No hace falta un puerto alternativo como solución permanente — cambiar de puerto enmascara el
síntoma sin resolver la acumulación, que seguirá creciendo si no se limpia.

**Cómo aplicar (rutina normal):** al terminar cualquier verificación en navegador que dependió
de un `run.py` propio arrancado por Claude en esta sesión:
1. `browser_close`.
2. `TaskStop` con el `task_id` del arranque — no dejarlo "por si acaso".

No aplica al servidor que Carlos arranca manualmente (`!` en el prompt) — ese lo gestiona él.

Ver también [[feedback_comandos_allowlist_verbatim]] (cómo escribir el arranque de `run.py`
para que la allowlist lo reconozca sin pedir permiso, incluida la ruta POSIX exacta).
