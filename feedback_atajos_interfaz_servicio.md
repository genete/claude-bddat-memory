---
name: feedback-atajos-interfaz-servicio
description: "Antes de añadir una función de conveniencia a la interfaz de un servicio, comprobar quién la llama y por qué; si el consumidor llega por el nivel equivocado, es navegación suya, no entrada del servicio"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 6f34100d-0515-4bb8-a776-65705a810f91
  modified: 2026-08-20T09:34:08.928Z
---

Al diseñar la interfaz pública de un servicio, no añadir entradas «de conveniencia» que
acepten un nivel que según el propio modelo no tiene esa propiedad. Antes de proponerlas,
mirar quién las llama y por qué llega con ese objeto en la mano.

Caso que lo originó (#778, 2026-08-20): propuse mantener una entrada «plazo de un trámite»
como atajo. Carlos lo cuestionó («¿Para qué el trámite? ¿atajo??»). Al comprobarlo, sus dos
únicos llamantes preguntaban lo mismo (`.estado == 'VENCIDO'`) y llegaban con un trámite
porque el vínculo con el organismo cuelga de ahí — no porque el trámite tenga plazo.

**Why:** la interfaz enseña el modelo. Una función llamada «plazo de un trámite» reintroduce
por la puerta de atrás un nivel que un rediseño acaba de eliminar, y quien la lea meses
después concluirá que ese nivel tiene la propiedad. El coste de repetir dos líneas de
navegación en el consumidor es menor que el de un modelo que se contradice a sí mismo.

**How to apply:** si el consumidor llega por el nivel equivocado, la bajada al nivel correcto
es una utilidad de navegación del árbol ESFTT, no una entrada del servicio de dominio.
Relacionado con [[feedback_umbral_factorizar_excepciones_bespoke]] y
[[feedback_invariante_vs_regla_motor]] — misma familia: decidir dónde vive un hecho, no solo
si funciona.
