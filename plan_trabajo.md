# Plan de Trabajo BDDAT — Feb 2026

Basado en: issue #61 (comentario 3863698363) + issue #93 + estado actual de develop.

## Estado general
Opción A Secuencial acordada en comentario #61:
1. ✅ #93 Fase 1-2 (UI base + backend)   → PRs #108 + #110 mergeados
2. ✅ #61 Fase 2 (#67 - Wizard)          → PR #112 mergeado
3. ✅ Bugs rápidos #109 y #113           → commits b051278 + 4bde7e8 en develop
4. 🔴 #93 Fase 3 (Refactoring modular)  ← PRÓXIMO SPRINT [2-3 días]
5. ⏳ #61 Fase 3 (#70-73 - Navegación)
6. ⏳ #93 Fase 4 (Metadata-driven)
7. ⏳ #61 Fase 4 (#85 - Accesos)

---

## Bloque siguiente: #93 Fase 3 — Refactoring Modular

### Objetivo
Migrar código actual a arquitectura modular preparada para metadata-driven.

### Estructura objetivo
```
app/modules/
├── expedientes/
│   ├── metadata.json
│   ├── routes.py
│   ├── templates/
│   └── static/
├── entidades/
│   └── ... (ídem)
└── __init__.py  ← ModuleRegistry (escaneo automático)
```

### Alcance inicial (solo expedientes y entidades)
- Modelos se quedan en app/models/ (NO se mueven)
- Resto de blueprints (auth, dashboard, etc.) se dejan en app/routes/ por ahora

### Preguntas a resolver antes de empezar
1. ¿Qué contiene metadata.json exactamente? (nav, permisos, iconos...)
2. ModuleRegistry: primero manual, luego auto-discovery
3. Limpiar templates legacy antes de migrar (ver lista abajo)

### Templates legacy a revisar/eliminar antes del refactoring
- base_full_width.html vs base_fullwidth.html (duplicado por nombre)
- expedientes/detalle.html, editar.html, nuevo.html, index.html (¿legacy pre-wizard?)

### Milestone "Alineación Administrativa" — Issues pendientes
| Issue | Tarea | Prioridad |
|-------|-------|-----------|
| #66 | Migraciones Alembic + tests modelos | media |
| #68 | Generación número AT tras validación completa | alta |
| #69 | Actualizar rutas Flask flujo creación integrado | alta |
| #70 | Listado Gestión Expedientes (datos SFTT condensados) | media |
| #71 | Menú lateral contextual (breadcrumb vertical) | media |
| #72 | Vista detalle con hijos (tabs + lista + acciones) | media |
| #73 | Breadcrumb horizontal + transiciones | media |
| #74 | Semáforos/alertas vencimientos dashboard | future |
| #75 | Búsqueda global | future |
| #76 | Exportación Excel/CSV | future |

---

## Convenciones recordadas
- NUNCA migraciones automáticas (flask db migrate) — bug Alembic con include_schemas
- Siempre migración manual: flask db revision → editar → flask db upgrade
- Tag antes de refactorings grandes (v0.4.0 antes de Fase 3)
