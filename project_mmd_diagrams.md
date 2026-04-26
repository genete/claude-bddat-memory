---
name: Diseño diagrama MMD de ESFTT
description: Decisiones de diseño sobre generación dinámica de diagramas Mermaid para visualizar la estructura ESFTT de un expediente
type: project
originSessionId: 84025c15-bdb8-4179-baf0-d2bb6dad4326
---
## API JSON agnóstica primero, MMD como capa posterior

**Decisión:** Crear `/api/expedientes/<id>/arbol` que devuelve JSON puro a 4 niveles (solicitud → fase → trámite → tarea), y sobre ese JSON construir serializadores independientes.

**Por qué no MMD directo:** El JSON del árbol es consumible por múltiples renderers (Mermaid, timeline, Excel, tests de integración). Acoplar Flask directamente a MMD sacrifica esa reusabilidad.

**How to apply:** Si se implementa, el endpoint devuelve JSON. El serializador MMD es una función Python independiente `arbol_to_mmd_*(arbol)`.

---

## Estrategia híbrida: dos diagramas Mermaid en la misma vista

Mermaid no permite mezclar tipos en un bloque. La solución son dos bloques independientes:

- **Vista macro** — `stateDiagram-v2`: mapa de fases del expediente, fase activa resaltada
- **Vista detalle** — `flowchart TD` con subgraphs: trámites y tareas de la fase seleccionada

Ambos beben del mismo JSON del árbol. El detalle se regenera al seleccionar fase.

**Serializadores:** `arbol_to_mmd_macro()` y `arbol_to_mmd_detalle(fase_id)`.

---

## Pendiente de documentar

Existe `docs/DISEÑO_DIAGRAMAS_ESFTT.md` (22/03/2026) con decisiones previas sobre capas 0-3.
El nuevo endpoint `/arbol` y la estrategia híbrida deben integrarse en ese documento.
El DISEÑO_ depende de `docs/ESTRUCTURA_FTT.md` como fuente de verdad estructural.
