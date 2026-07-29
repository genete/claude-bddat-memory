---
name: feedback-enganche-temprano-vs-tardio
description: "Ante dos puntos de enganche candidatos para automatizar sobre un documento, preferir el más temprano si el dato disparador ya se fija ahí"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 6f9edf1d-c16a-4218-95ae-81e64009f9d9
  modified: 2026-07-20T12:35:11.532Z
---

Cuando una automatización (parseo, autorrelleno) puede engancharse en más de un punto del ciclo de vida de un documento, preferir el punto más temprano en el que el dato que dispara la lógica (p. ej. `tipo_doc_id`) ya es conocido, en vez de asumir que un punto posterior (p. ej. la asociación a una tarea) es el lugar "natural" solo porque es donde se consume el resultado.

**Caso concreto (#657):** propuse enganchar el autorrelleno del parser de justificante de notificación en la asociación del documento a la tarea NOTIFICAR (enganche 2), razonando que ahí es donde se necesita el resultado. Carlos corrigió: el tipo de documento (`tipo_doc_id`) ya es obligatorio y se fija en el momento de subida al pool — es el primer punto donde se conoce que un fichero es `JUSTIFICANTE_NOTIFICA`, así que es ahí donde debe dispararse el parser, no más tarde. Su argumento adicional: si el documento no estuviera ya clasificado antes de llegar al selector de la tarea, el sistema no podría ni ofrecerlo como candidato — la clasificación siempre precede a la asociación.

**Por qué:** en sistemas donde un campo se fija de forma temprana y obligatoria (aquí, el tipo de documento en la subida), ese campo es el trigger más fiable y más temprano disponible — enganchar más tarde solo añade latencia entre "el sistema ya sabe que podría autorrellenar esto" y "lo hace".

**Cómo aplicar:** al diseñar dónde insertar lógica condicional sobre un dato ya existente en el sistema, mapear en qué punto exacto ese dato se fija por primera vez y de forma obligatoria, y enganchar ahí — no en el punto donde el resultado se consume o se muestra, salvo que el dato disparador no esté disponible todavía en el punto temprano.
