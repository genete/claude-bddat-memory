---
name: subsistema_documental
description: Decisiones arquitectónicas del subsistema documental (pool, tipos, vías de entrada)
type: project
---

Detalle completo: `docs/fuentesIA/ARQUITECTURA_DOCUMENTOS.md`

**Decisiones clave:**
- **Pool agnóstico:** `Documento` solo tiene FK `expediente_id` y `tipo_doc_id`. No añadir FKs directas.
- **URLs ficheros subidos:** `UPLOAD_FOLDER/expedientes/<id>/<uuid>/<nombre>` → ruta relativa en `Documento.url`.
- **Particularización N:M** (NO herencia SQLAlchemy) para extender documentos. Precedente: `DocumentoProyecto`.
- **`fecha_administrativa` nullable:** NULL = pendiente de revisión o sin valor jurídico (borrador/informe interno).
- **Nombre a mostrar:** calculado desde URL (último segmento sin extensión). No campo propio.

**Dos vías de entrada al pool:**
- Vía 1 — Carga masiva: administrativo carga al pool. No genera tareas ESFTT.
- Vía 2 — Tarea INCORPORAR: documentos externos durante tramitación activa. El doc debe existir ya en pool.

**RECEPCION_SOLICITUD:** usa patrón ANALISIS (patrón A). El técnico verifica pool y cualifica docs con `tipo_doc_id`.

**ZIP:** no válido como unidad de trabajo — solo referencia histórica.

**Why:** Separación entre gestión masiva (soporte administrativo) y tramitación ESFTT (actos con trazabilidad).
