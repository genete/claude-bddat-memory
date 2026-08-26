---
name: feedback_urgencia_desarrollo_vs_produccion
description: "BDDAT está en desarrollo, no en producción — no dar a los bugs una urgencia de \"hay que arreglarlo antes que nada\" salvo que el propio Carlos lo pida"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 10075e52-5c11-48ac-9010-bf6690a39bd1
  modified: 2026-08-15T08:43:03.469Z
---

Un bug real en `plazos.py` (#778: expedientes que nunca vencen) es **preceptivo antes
de producción**, pero eso no significa que haya que resolverlo primero ni con prisa de
incidente. BDDAT no está desplegado — no hay expedientes reales sufriéndolo hoy. Carlos
lo señaló explícitamente el 2026-08-15: la urgencia que yo transmitía ("es el ítem #1 de
la cola, hay que fijarlo ya") venía de una calibración de protección de sistemas en
producción, y aquí sesga el análisis porque no aplica.

**Por qué:** en desarrollo, "urgente" se mide contra el calendario de salida a
producción, no contra un "ahora mismo". Es perfectamente correcto diferir un bug
importante si arreglarlo ahora significa escribir datos o código contra un diseño que
va a cambiar (ver #784, catalogo_plazos) — arreglarlo una vez bien gana a arreglarlo dos
veces.

**Cómo aplicarlo:** al priorizar o argumentar por qué algo debe hacerse ya, no dar por
hecho un marco de "esto puede estar afectando a usuarios reales ahora". Preguntar o
verificar si BDDAT ya está en producción antes de aplicar ese framing. Sí es válido
señalar el coste de diferir (para que la decisión se tome con los ojos abiertos), pero
sin presentarlo como argumento decisivo por sí solo.
