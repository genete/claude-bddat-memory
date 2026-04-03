---
name: Listado inteligente — decisiones pendientes antes de implementar vista
description: Decisiones de diseño que hay que tomar antes de implementar el paso 7 del §9 de ANALISIS_LISTADO_INTELIGENTE.md (vista del listado, issue #169)
type: project
---

Seguimiento.py ya implementado (PR #261). El siguiente paso es la vista, pero requiere decidir:

## 1. Ruta

¿Reemplazar la `/expedientes/` existente (ruta `/` del módulo expedientes) con el listado inteligente, o añadir una ruta nueva (ej. `/expedientes/listado/`) dejando convivir ambas temporalmente?

## 2. Filtro por usuario

- Por defecto: ¿mostrar solo los expedientes del usuario actual (`responsable_id = current_user.id`) o todos?
- Propuesta: por defecto `mis` expedientes, toggle para `todos` (supervisores)
- Reflejado en URL: `?ver=mis` / `?ver=todos`

## 3. Filtro activos/resueltos

El análisis dice activos por defecto.
- Propuesta: `?estado=activos` (default) / `?estado=resueltos` / `?estado=todos`

**Why:** Estas decisiones afectan la arquitectura de la vista y la query principal antes de escribir una sola línea de código.
**How to apply:** Al arrancar la sesión de implementación del listado, preguntar al usuario si ya tiene respuestas antes de empezar.
