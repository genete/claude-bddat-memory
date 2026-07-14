---
name: project-fusion-n012-n013
description: Carlos considera artificiosa la distinción N012 (Tramitador)/N013 (Administrativo) del catálogo de necesidades — pendiente fusionar
metadata: 
  node_type: memory
  type: project
  originSessionId: a967a69f-43bb-48ec-8292-91959fc8aa17
---

Al decidir el issue de enganche del modal de generación de escritos (2026-07-10), Carlos
señaló que N012 ("Generar escrito desde plantilla y descargar versión borrador/firmada",
rol Tramitador) y N013 ("Generar escritos estándar y avanzar tramitación", rol
Administrativo) son en el fondo la misma necesidad — la distinción por rol es artificiosa.
Mismo backend, mismo hueco de UI, mismo % de cobertura en `MATRIZ_COBERTURA_BDDAT.md`.

**Por qué:** no hay diferencia funcional real entre ambas entradas del catálogo; solo el
rol que las consume difiere, y eso no justifica dos necesidades separadas.

**Cómo aplicar:** de momento (issue de enganche) se etiquetan ambas (`necesidad:N012` +
`necesidad:N013`) sin fusionar. Pendiente una sesión futura que fusione N012/N013 en
`DETALLE_NECESIDADES_BDDAT.md` y `MATRIZ_COBERTURA_BDDAT.md` (documentos vivos — no
unilateral, requiere confirmación de Carlos igual que cualquier edición de esos ficheros).
No abrir esa fusión como efecto colateral de otro issue; es un cambio de catálogo aparte.
