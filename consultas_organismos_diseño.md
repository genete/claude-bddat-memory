---
name: Diseño fase consultas organismos y análisis técnico
description: Decisiones de arquitectura tomadas para la fase de consultas a organismos y análisis técnico. Documento completo en docs/fuentesIA/DISEÑO_CONSULTAS_ORGANISMOS.md
type: project
---

Diseño cerrado el 21/03/2026. Documento de referencia: `docs/fuentesIA/DISEÑO_CONSULTAS_ORGANISMOS.md`.

**Why:** La fase de consultas tiene cardinalidad variable (N organismos) y la fase de análisis técnico tiene requerimientos de longitud indefinida. Se necesitaba un modelo que no rompa el principio de minimalismo ni obligue a duplicar datos.

**How to apply:** Leer el documento antes de implementar cualquier cosa relacionada con la fase de consultas o análisis técnico.

## Decisiones clave

- **`organismos_expediente`**: tabla única (no dos tablas afectados/consultados). Campo `via` distingue `consulta` vs `declaracion_responsable`. El estado del organismo es lo que comprueba el motor, no los trámites individuales.
- **Requerimientos**: sin tabla propia. Son trámites ordinarios (`REQUERIMIENTO_DE_MEJORA` + `COMPROBACION_DOCUMENTAL`). El tramitador gestiona la correspondencia con numeración y notas.
- **Tres tipos de trámite** para consultas: `CONSULTA_SEPARATA`, `CONSULTA_TRASLADO_TITULAR`, `CONSULTA_TRASLADO_ORGANISMO`. Compartidos entre AAP, AAC y DUP.
- **SEPARATA en bloque**: una acción genera N documentos y N trámites, uno por organismo.
- **Cadena de tareas** en traslados: REDACTAR→FIRMAR→NOTIFICAR→ESPERAR→INCORPORAR_RESPUESTA→ANALIZAR. La tarea ANALIZAR es productora del resultado semántico que determina el siguiente paso.
- **DUP**: siempre al unísono con la autorización principal. Una sola separata cubre todos los efectos. Art. 146.2 exime la ronda DUP si se siguió art. 127.

## Reglas del motor para cierre de fases
- **Consultas**: todos los `organismos_expediente` en estado terminal.
- **Análisis técnico**: ningún `REQUERIMIENTO_DE_MEJORA` con INCORPORAR pendiente + ninguna `COMPROBACION_DOCUMENTAL` sin resultado OK.
