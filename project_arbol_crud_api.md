---
name: project-arbol-crud-api
description: "El CRUD del árbol del expediente vive en api_expedientes.py (/nodo/...), no en api_bc.py (muerto desde"
metadata: 
  node_type: memory
  type: project
  originSessionId: 3dce9da2-c9c0-4a7d-b566-a2f6d1643ad0
---

La isla React del árbol (`expediente-arbol`) hace TODAS sus mutaciones contra
`/api/expedientes/<id>/nodo/<tipo>/<id>` — GET detalle, GET `/editable`, PATCH (editar),
POST `/hijos` (crear), DELETE (borrar) — definidos en `app/routes/api_expedientes.py` y
consumidos desde `react-src/src/expediente-arbol/api.js`.

`app/routes/api_bc.py` (la antigua API de las vistas BC, #314) está **muerta**: ningún
template ni JS consume `/api/bc/` desde que se eliminaron las vistas BC (#519), aunque el
blueprint sigue registrado en `app/__init__.py`.

**Al refactorizar permisos o comportamiento del árbol, tocar `api_expedientes`, NO `api_bc`.**
En #501 el análisis de impacto apuntó a `api_bc` por el texto del issue/ADR y el cambio de
permisos no tuvo efecto en el árbol (el admin recibía 405/flash colgado). Lección: verificar
el consumidor real (`react-src/.../api.js`) antes de fiarse del nombre de API citado en el
issue. Relacionado: [[feedback_analisis_impacto]].
