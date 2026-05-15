---
name: feedback-analisis-impacto
description: "Ante refactorizaciones o cambios de diseño, presentar tabla de consumidores antes de implementar"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 4dbf03d1-e8af-4a1b-8448-628bd59ee064
---

Antes de implementar cualquier cambio de diseño o refactorización que elimine o cambie el contrato de un concepto (tabla, modelo, servicio, función, endpoint, verbo, script...), presentar al usuario una tabla con TODOS los consumidores en TODO el sistema y esperar confirmación.

**Why:** Durante #387 (eliminación whitelists) y antes en ADR-003, se dejó código zombie (`reset_maestros_ftt.py`) porque el análisis se limitó a `app/` y no cubrió `scripts/`, `*/README.md` ni prerequisitos documentados entre scripts.

**How to apply:** Buscar en todas las capas (`app/`, `tests/`, `scripts/`, `migrations/`, `docs/*/*`, `*/README.md`, docstrings) antes de escribir una sola línea de código. Clasificar cada consumidor como Actualizar / Eliminar / Dejar (zona congelada). Presentar la tabla al usuario y esperar confirmación explícita. Ver §"Análisis de impacto previo" en `docs/guias/REGLAS_DESARROLLO.md`.
