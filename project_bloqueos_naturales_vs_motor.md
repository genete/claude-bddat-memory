---
name: bloqueos-naturales-vs-motor
description: "Distinción entre bloqueo por motor de reglas y bloqueo natural por falta de documento de entrada, al diseñar el enganche entre fases"
metadata: 
  node_type: memory
  type: project
  originSessionId: ee79230a-c6d0-4cf8-b130-497b5a321c66
---

Al diseñar cómo una fase/trámite posterior queda bloqueado por defectos de ANÁLISIS_SOLICITUD, hay dos mecanismos distintos que no deben confundirse ni resolverse con la misma pieza:

- **Bloqueo de motor real**: la tasa impagada bloquea *toda* actuación administrativa posterior (art. 45.1 Ley 10/2021), aunque todo lo demás esté completo. Esto sí necesita una `ReglaMotor` — ninguna tarea es irrealizable por sí sola solo porque falte la tasa.
- **Imposibilidad natural por falta de documento**: si no hay separata presentada, CONSULTA_SEPARATA no tiene documento que consumir en su ELABORAR — la tarea es irrealizable por construcción, no hace falta ninguna regla de motor que lo impida explícitamente. Mismo caso para el EIA/tasa medioambiental de cara a AAU_AAUS_INTEGRADA.

**Por qué importa:** en la sesión de análisis de #408/#440/#442/#495/#581 (2026-07-03) propuse tratar los tres ejemplos (tasa, separata, EIA) como un único issue de "bloqueos cruzados de fase"; Carlos corrigió que son dos naturalezas distintas y que agruparlos hubiera sido una generalización incorrecta.

**Cómo aplicar:** al modelar el bloqueo de una fase por falta de un documento/requisito, primero preguntar si la tarea consumidora ya es irrealizable sin ese documento (caso natural, no requiere trabajo de motor) antes de proponer una `ReglaMotor` nueva.

**Aparcado:** el caso EIA → AAU_AAUS_INTEGRADA se retoma cuando se abra el hilo de diseño de esa fase, no antes — analizarlo ahora "quedaría cojo" (contexto insuficiente).
