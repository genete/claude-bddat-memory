---
name: feedback-no-inventar-aislamiento-sin-ruta-herencia
description: No proponer mecanismos de aislamiento entre datos ficticios y reales sin verificar antes si existe una ruta real de herencia entre entornos
metadata: 
  node_type: memory
  type: feedback
  originSessionId: a1884c25-d951-4c6c-87a1-7fd95f0777d5
---

Al diseñar el dataset ficticio de BDDAT (ADR-030), propuse dos veces un mecanismo de aislamiento entre catálogo ficticio y real para evitar "contaminar" producción: primero un rango de NIF/numero_at reservado, luego un vocabulario de tipos_fases/tramites/solicitudes con prefijo TEST_* separado del real. Carlos corrigió ambas veces con el mismo argumento: BDDAT nunca promociona ni hereda datos de desarrollo hacia producción — producción se siembra aparte, de cero, bajo control explícito del Supervisor en el momento del despliegue. Sin ruta de copia/herencia entre entornos, no hay nada que aislar.

**Why:** Estaba aplicando un reflejo genérico de buenas prácticas (aislar datos de prueba de datos de producción) sin verificar si la premisa que lo justifica —que el dato de dev pueda acabar en producción por algún mecanismo automático— era cierta en este proyecto. No lo es, y proponer el aislamiento dos veces fue sobre-ingeniería que Carlos tuvo que descartar explícitamente cada vez.

**How to apply:** Antes de proponer cualquier mecanismo de aislamiento, marcado o separación entre datos de entornos distintos (dev/test vs. producción, ficticio vs. real), verificar o preguntar explícitamente si existe alguna ruta real de copia/promoción/herencia entre esos entornos. Si no la hay, el aislamiento no aporta nada.
