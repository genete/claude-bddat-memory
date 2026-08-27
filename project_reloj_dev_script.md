---
name: project_reloj_dev_script
description: "scripts/reloj_dev.py cambia la fecha simulada de #820 sin bootstrap de Flask; vocabulario natural de Carlos → subcomando exacto"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 8148f138-ee04-470b-9e09-7b077cfcc624
  modified: 2026-08-27T06:20:38.498Z
---

`scripts/reloj_dev.py` — reloj de desarrollo (#820): lee/escribe directamente
`instance/reloj_simulado.txt` (el mismo fichero que `_hoy()` en
`app/services/plazos.py`), sin arrancar Flask. Usar siempre este script para
cambiar la fecha durante una sesión — nunca `flask reloj` (arranca la app
entera + valida catálogo + `SQLALCHEMY_ECHO` en cada llamada, mucho más lento
si hay que cambiar de fecha muchas veces seguidas).

Comando: `venv/Scripts/python.exe scripts/reloj_dev.py <subcomando>`

Vocabulario de Carlos → subcomando (para entenderlo sin pestañear):
- "aumenta / avanza / suma N días [hábiles]" → `avanzar N` (añadir `--habiles`
  si dice "hábiles" / "laborables")
- "retrocede / resta / quita N días [hábiles]" → `retroceder N` (+ `--habiles`
  si aplica)
- "pon / fija la fecha a AAAA-MM-DD" → `set AAAA-MM-DD`
- "vuelve a hoy / quita la simulación / resetea el reloj" → `reset`
- "qué fecha hay puesta" → `show`

`avanzar`/`retroceder` parten siempre de la fecha simulada activa (o de la
fecha real si no hay ninguna fijada) — son encadenables, cada llamada mueve
el fichero desde donde quedó la anterior. `--habiles` consulta la tabla
`dias_inhabiles` directamente vía `psycopg2` (lee `DATABASE_URL` de `.env`,
sin pasar por el ORM) y reutiliza el mismo algoritmo que `_es_habil()` /
`_medir()` en `plazos.py`, así que el conteo de festivos coincide exactamente
con el que usa el motor de plazos real.

El script solo toca el fichero — no hace falta que Flask esté arrancado para
cambiar la fecha. Para ver el efecto en el navegador sí hace falta el
servidor corriendo (ver [[feedback_matar_proceso_flask_al_cerrar_navegador]]
para pararlo correctamente al terminar la verificación).
