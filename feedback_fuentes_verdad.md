---
name: feedback-fuentes-verdad
description: Distinguir fuentes de verdad de derivados en BDDAT; no tratar derivados como consumidores independientes en análisis de impacto
metadata: 
  node_type: memory
  type: feedback
  originSessionId: ddfcdf30-d906-4cf0-926a-0b16c3eefbea
---

En el análisis de impacto de cambios estructurales, NO clasificar los documentos derivados como consumidores independientes que "hay que editar". Son consecuencia de la fuente, no consumidores del cambio.

**Fuentes de verdad del proyecto (de REGLAS_ARQUITECTURA.md §2.1):**
- `ESTRUCTURA_FTT.json` — fuente de verdad estructural ESFTT para código e IA
- `NORMATIVA_*.md` — fuente de verdad normativa externa

**Derivados conocidos (solo necesitan sincronización cuando cambia su fuente):**
- `ESTRUCTURA_FTT.md` ← deriva de `ESTRUCTURA_FTT.json`
- `ANALISIS_TAREAS_INVERSO.md` ← deriva de `ESTRUCTURA_FTT.json`
- `ANALISIS_LISTADO_INTELIGENTE.md` ← deriva de `ESTRUCTURA_FTT.json`
- `ANALISIS_GENERACION_DIAGRAMA_EXPEDIENTE.md` ← deriva de `ESTRUCTURA_FTT.json`
- `DISEÑO_FECHAS_PLAZOS.md` ← deriva de `NORMATIVA_PLAZOS.md`

**Why:** En el análisis de #366, se clasificaron `ANALISIS_TAREAS_INVERSO.md` y `ESTRUCTURA_FTT.md` como si fueran documentos independientes que "hay que editar". El usuario corrigió: si el JSON (fuente) ya es correcto, los derivados no son consumidores del issue — simplemente necesitan sincronizarse mediante `/sync-derivados`.

**How to apply:** En el mapa de consumidores de un análisis de impacto, separar siempre:
1. Fuentes de verdad afectadas → requieren decisión de diseño
2. Código/checks/migraciones → consumidores reales que hay que actualizar
3. Documentos derivados → sincronizar tras el cambio, no editar como consumidores independientes

Ante cualquier duda sobre si un fichero es fuente o derivado, leer `docs/historial/REGLAS_ARQUITECTURA.md §2.1`.
