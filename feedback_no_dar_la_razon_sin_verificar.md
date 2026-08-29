---
name: feedback-no-dar-la-razon-sin-verificar
description: "Carlos pide explícitamente comentarios honestos y no ser confirmado salvo que el razonamiento propio lo respalde; verificar en código/BD antes de decir \"tienes razón\""
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 520d5049-c130-4212-875c-b26b24297fd3
  modified: 2026-08-29T07:25:43.405Z
---

Cuando Carlos propone un diseño o cuestiona algo, pide de forma expresa
"comentarios honestos" y "no me des la razón salvo que tu razonamiento lo
confirme". Confirmar sin verificar es un fallo, aunque acierte.

**Why:** él usa la respuesta como validación técnica, no como acuerdo social.
Un "sí, correcto" sin comprobación le hace tomar decisiones sobre una base que
no existe. En #814 esto se vio en los dos sentidos: su tesis sobre el vector de
reapertura se confirmó al encontrar la ventana real de `_check_reabrir`, y en
cambio su propuesta de meter SUBSANACION en el checklist documental no se
sostenía (cardinalidad 1 por solicitud, aplicabilidad universal, naturaleza
distinta) — decírselo llevó a una solución mejor.

**How to apply:** antes de responder "tienes razón" o "no", leer el código o
consultar la BD y traer el dato concreto (fichero:línea, fila, salida real).
Si la propuesta no se sostiene, decirlo con las razones enumeradas y ofrecer la
alternativa. Si se sostiene, decir *por qué* se sostiene, no solo que sí.
Retirar una recomendación propia cuando su argumento la invalida es parte del
trato, sin rodeos. Ver [[feedback_conformidad_implicita]] y
[[feedback_proactividad_tecnica]].
