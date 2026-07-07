---
name: feedback-mapear-todo-aunque-implementacion-parcial
description: "Al planificar infraestructura de tests o cobertura de cara a un hito, mapear todos los mecanismos que necesitan verificación aunque la implementación de algunos quede como placeholder"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: a1884c25-d951-4c6c-87a1-7fd95f0777d5
---

Al diseñar ADR-030 (dataset ficticio + matriz de cobertura del motor de reglas/plazos), propuse dejar fuera del ADR la pieza de generación de documentos y gestión física de ficheros, para no disparar el alcance, ofreciendo tratarla en un ADR complementario posterior. Carlos corrigió: el objetivo del ADR no es solo utilidad de desarrollo, es una auditoría de completitud de cara a la puerta de producción M4→M5 — quiere el mapa completo de qué mecanismos hay que poder verificar antes de entrar en producción, aunque la implementación de partes de ese mapa quede como placeholder o test con skip documentado. Cita textual: "el diseño de la suite de tests, librerías etc. debe contemplarlo todo aunque deje espacios sin cubrir... Hacer el análisis parcial es un tiro en el pie."

**Why:** Recortar el ANÁLISIS por anticipar que la IMPLEMENTACIÓN completa sería grande oculta huecos que van a aparecer de todos modos más adelante, con más coste de descubrirlos entonces (ya en producción o cerca) que de identificarlos ahora aunque no se resuelvan todavía.

**How to apply:** Al planificar un ADR de infraestructura, una suite de tests, o cualquier análisis de completitud de cara a un hito/milestone de producción, no acotar el alcance del ANÁLISIS asumiendo que la implementación será parcial. Es correcto priorizar y dejar partes como placeholder/diferido en la IMPLEMENTACIÓN — no lo es dejarlas fuera del MAPA. Relacionado: [[feedback_proactividad_tecnica]].
