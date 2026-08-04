---
name: feedback_puertos_zombis_windows_run_py
description: "Windows permite varios procesos run.py escuchando a la vez en el mismo puerto sin dar error — si un 500/404 persiste tras 'reiniciar' el servidor, sospechar acumulación de procesos antes que bug de código"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: f85ae007-73f5-42aa-9677-7d59cf6de3c3
  modified: 2026-08-04T12:32:31.732Z
---

Windows **no** exige `SO_EXCLUSIVEADDRUSE` para el `bind()` de un socket TCP — a diferencia
de Linux, varios procesos `python run.py` pueden quedar **todos** escuchando en el mismo
puerto (p. ej. 5000) sin que ninguno arranque con error de "puerto ocupado". El SO enruta
peticiones de forma impredecible entre ellos (aparentemente favorece el más antiguo). Cada
arranque nuevo se ve limpio en su propio log — el síntoma es que el navegador sigue viendo el
comportamiento del proceso viejo (p. ej. un blueprint que no existía antes de un cambio de
código reciente) por más que se "reinicie" el servidor.

**Por qué (sesión #630, PR #756, 2026-08-04):** tras mergear el hub `seguimiento_y_huerfanos`,
la pestaña Huérfanos daba `500 BuildError: api_huerfanos.listar_huerfanos` de forma
consistente en el puerto 5000, sobreviviendo a múltiples "reinicios" (tanto vía Bash
`run_in_background` como vía `scripts/flask_console.py`) y a limpiar caché de navegador. Se
investigó código duplicado, caché de bytecode `.pyc`, `PYTHONPATH`, exclusión de puertos
Hyper-V/WSL — todo descartado. La causa real: `Get-CimInstance Win32_Process -Filter
"Name='python.exe'" | Where CommandLine -like '*run.py*'` reveló **14 procesos** acumulados
desde las 13:53 (uno o dos por cada intento de arranque de toda la sesión), ninguno realmente
muerto pese a varios `taskkill`/`Stop-Process` previos que reportaban éxito. Al matarlos todos
a la vez y arrancar uno solo, el error desapareció al instante.

**Cómo aplicar:**
1. Si un error 500/404 persiste de forma idéntica tras "reiniciar" el servidor de desarrollo
   (Bash `run_in_background`, `flask_console.py`, o cualquier otro), **antes** de sospechar del
   código: `Get-CimInstance Win32_Process -Filter "Name='python.exe'" | Where-Object
   { $_.CommandLine -like '*run.py*' } | Select ProcessId,CreationDate,CommandLine` (PowerShell,
   vía `mcp__windows-mcp__PowerShell` — más fiable que `netstat`/`taskkill` desde Git Bash, que
   en esta sesión dio PIDs fantasma inconsistentes entre llamadas sucesivas).
2. Si aparece más de un proceso, matarlos **todos** de golpe (`Stop-Process -Id <id> -Force`
   por cada uno) antes de arrancar uno nuevo — matar solo "el último" no basta.
3. Verificar limpio antes de reintentar: `Get-NetTCPConnection -LocalPort <puerto>` sin filas,
   y el conteo de procesos `run.py` a cero.
4. Preferir `Get-CimInstance Win32_Process`/`Get-Process` vía PowerShell (`windows-mcp`) sobre
   `netstat -ano`/`taskkill` vía Git Bash para diagnóstico de procesos — en esta sesión el
   segundo camino dio resultados contradictorios (PIDs que se reportaban muertos seguían
   apareciendo como dueños de conexiones `Listen`/`Established`).

No es necesario un puerto alternativo como solución permanente (5050 funcionó de inmediato en
esta sesión precisamente porque nunca había acumulado procesos) — cambiar de puerto enmascara
el síntoma sin resolver la acumulación en 5000, que seguirá creciendo en sesiones futuras si no
se limpia.
