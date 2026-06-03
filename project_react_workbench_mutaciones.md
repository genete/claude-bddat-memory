---
name: project_react_workbench_mutaciones
description: Decisión de arquitectura del revamping — isla React workbench multiuso + mutaciones en servicio reutilizable
metadata: 
  node_type: memory
  type: project
  originSessionId: 6c02de24-e00d-43cf-82de-c32cd0393c5e
---

Decidido en sesión de diseño (2026-06-01, contexto #500 S3b) con Carlos:

- El combo **viewbar–main–inspector** (inspector = vista / editor / despensa) es la isla React
  "**workbench**" multiuso: vale para el árbol del expediente, listados seleccionables y la futura
  vista de **proyecto** (misma idea: jerarquía pintada + despensa + editor del detalle). Se construye
  **genérico** (`nodo = {tipo, id}`, campos genéricos, esquema editable genérico, fuente de despensa
  genérica), NO clavado a los conceptos del árbol → reutilizable sin tocar el front.

- **Mutaciones (Crear/Editar/Borrar) = camino B limpio**: lógica canónica en `services/` (extraída,
  no reescrita, de `api_bc`), endpoints JSON finos del árbol, y `api_bc` pasa a **delegar**. Una sola
  fuente de verdad, reutilizable por proyecto. Regla: **extraer, no reescribir** (corrección sutil ya
  peleada: stubs transientes del motor, cascada de borrado, invariante de cierre de fase, hook #458,
  bitácora). Razón para asumir el coste: no hay uso real con datos ("no producción"), las vistas BC
  se deprecan al cerrar #500, y proyecto reusará el servicio.

- **Esquema editable backend-driven y genérico**: un GET devuelve `[{campo, etiqueta, control, valor,
  opciones?}]`; el front pinta el formulario sin conocer el modelo (misma filosofía que `campos` de
  `detalle_nodo`).

- **Convivencia barata Jinja + React caro donde rinde.** La decisión "archipiélago vs React-total"
  NO se toma: se acepta híbrido con React solo donde la riqueza de interacción lo justifica. Si algún
  día se formaliza la capa React compartida (tokens + librería de componentes + un runtime para el
  workbench), será un **ADR aparte post-#500**. La duplicación de componentes solo muerde en widgets
  con comportamiento JS imperativo; los controles planos comparten clases Bootstrap (sin duplicar).

Relacionado: [[project_estado_mayo2026]], [[project_backend_solido_revamping]].
