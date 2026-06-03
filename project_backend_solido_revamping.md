---
name: project_backend_solido_revamping
description: "El backend ESFTT (motor agnóstico + assembler + properties deducidas) soporta las islas del revamping con casi pura lectura; estimar por lo que hay que construir, no por lo que la vista aparenta"
metadata: 
  node_type: memory
  type: project
  originSessionId: 8a97cc15-fd8a-46b3-abaf-ffb7d3ea699a
---

Al implementar el backend de la vista de árbol (#500, ADR-016), se confirmó que el core técnico ya construido absorbe el trabajo pesado de las vistas nuevas:

- El **motor de reglas agnóstico** + **`assembler.py`** ya soportaban evaluar candidatos hipotéticos (`objeto={'solicitud': s, 'tipo_fase': tf}`), tipos combinados (AAP+AAC vía `evaluar_multi`) y el modo global. El endpoint `tipos-creables` fue "enumerar candidatos × `evaluar_multi`", no lógica nueva.
- Las **properties de estado deducidas** (`Solicitud.estado`, `Fase.estado`, `seguimiento.py`, `plazos.py`) hicieron que `/arbol` fuese casi pura serialización.
- Los **agregadores de subárbol** reutilizan el patrón de recorrido abajo-arriba de `seguimiento.py`.

**Por qué importa:** al planificar las islas siguientes (S2/S3 de #500, luego #501 "Mi trabajo", #502 Command Palette), **medir el coste por lo que hay que *construir* leyendo el dominio existente, no por lo que la vista *aparenta***. Un bloque que parece de días puede ser un consumo fino del backend ya hecho. Verificar siempre primero qué existe (motor, assembler, services, properties) antes de estimar.

Detalle de diseño confirmado: el sujeto ESFTT (`'Distribucion/AAP/RESOLUCION'`) **no tiene segmento de tarea** → las tareas se rigen por el patrón FTT (`tramites_tareas`), no por reglas normativas del motor. Coherente con el diseño del dominio. Ver [[project_estado_mayo2026]].
