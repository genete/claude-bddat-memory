---
name: project-estado-mayo2026
description: "Snapshot de estado del proyecto BDDAT a mayo 2026 — qué está hecho, cuellos de botella y huecos sin issue"
metadata: 
  node_type: memory
  type: project
  originSessionId: 15f65b94-db9d-4680-89d4-33a68d971bfa
---

Análisis de situación realizado el 2026-05-13 por lectura directa de código + issues. Documento completo en `docs/historial/ANALISIS_ESTADO_MAYO_2026.md`.

**Why:** Punto de referencia para futuras sesiones sobre priorización de trabajo y planificación de milestones.

**How to apply:** Antes de decidir qué abordar en el camino a producción, contrastar con este snapshot para no reinventar el análisis.

---

## Hallazgos clave (no obvios desde el código superficial)

- El core técnico está construido: motor de reglas con excepciones, plazos con suspensiones (art. 22 LPACAP), generador de escritos con subdocs y corrección ZIP, API BC con CRUD completo de ESFTT integrado con el motor.
- El estado de tarea/trámite se **deduce** del árbol documental — no hay campo `estado` explícito (ADR-002). Las acciones iniciar/finalizar son validaciones sin mutación; la mutación real es vía `editar_tarea`.
- **Cuello de botella real:** datos de catálogo ESFTT incompletos normativamente (whitelists E→S, S→F, F→T), Context Builders (Capa 2) de escritos sin implementar (#289, M2), e infraestructura de producción sin tocar (M4).

## Huecos sin issue en GitHub (a esta fecha)

1. Semáforos de plazo en la vista BC de tramitación (el #74 solo cubre el dashboard, M5)
2. UI de configuración del catálogo de plazos para el Supervisor
3. Vista de asignación masiva de expedientes por Supervisor
4. Elementos técnicos del proyecto (líneas AT, CT, subestaciones) — sin diseño ni issue

## Issues cuya clasificación era cuestionable

- #379 (Tipos documentos en UI): estaba en M4, debería ser M2 o anterior
- #74 (Semáforos): estaba en M5, probablemente M2
- #376, #374, #344: sin milestone asignado
