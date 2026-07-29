---
name: project-adr034-notificaciones-tarea-id
description: "ADR-034 corrige ADR-008 — notificaciones ancla por tarea_id, no vitaminado de documento; diseño final de #657/#658"
metadata: 
  node_type: memory
  type: project
  originSessionId: 6f9edf1d-c16a-4218-95ae-81e64009f9d9
  modified: 2026-07-21T06:52:25.729Z
---

`docs/decisiones/ADR-034-ciclo-vida-notificacion-dos-documentos.md` (adoptada
2026-07-20, commit `563e12c`) corrige el encuadre de ADR-008 tras planificar
#657/#658: el acto de notificar por Notifica-PNT produce **dos justificantes**
(puesta a disposición + definitivo, mismo formato de PDF, descargado dos veces —
confirmado con muestra real, ver más abajo), y `notificaciones` no es un
vitaminado de documento (ADR-005) sino una **tabla de seguimiento del acto de
notificar, anclada a `tarea_id`** — `resultado`/`numero_intento` son mutables,
un documento no lo es.

**Schema final:** `tarea_id NOT NULL` (ancla real), `documento_id` pasa a
nullable (sigue `UNIQUE`), `resultado` nullable, `identificador_envio VARCHAR`
(campo único genérico para los 4 canales, no uno por canal), fecha partida en
`fecha_puesta_disposicion NOT NULL` + `fecha_resultado` nullable (antes
`fecha_notificacion` única — sin consumidores reales, rename seguro).

**El justificante intermedio nunca es un `Documento`** — no se sube al pool ni
se guarda en disco (LPACAP no da cabida a una pieza que duplica al definitivo
sin aportar nada distinto; el equipo real raramente lo descarga, solo usa la
remesa). Se parsea de forma transitoria desde una acción "registrar envío" en
`NotificarEditor` (tarea NOTIFICAR), y se descarta.

**Dos caminos de escritura:** A (registro temprano opcional, sin documento) y
B (definitivo — la creación/upsert real vive en el hook de `editar_tarea` al
fijar `documento_producido_id`, buscando por `tarea_id`, NO en la subida al
pool — ahí el documento aún no está vinculado a ninguna tarea). El cotejo
anti-expediente-equivocado (motivo original de #658) sale gratis de este
diseño: compara `identificador_envio` del camino A contra el del documento
definitivo, sin ningún paso manual nuevo.

**Generalización a los 3 canales con datos reales:**
- NOTIFICA: parser completo (#655), cotejo automático.
- BANDEJA: un solo acto rellena las dos fechas a la vez (entrega instantánea al
  firmar/enviar — el estado `PENDIENTE` de asignación interna en destino es
  irrelevante para LPACAP). Parseable en principio, sin muestras reales aún —
  fuera de alcance de #657/#658, puerta abierta.
- SIR (ARIES): sin justificante descargable (solo captura de pantalla) — 100%
  manual, sin fecha de automatización prevista. `NotificarEditor` tiene que
  funcionar como formulario manual puro por defecto, con el parser como mejora
  opcional encima (hoy solo NOTIFICA).

**Verificado con muestra real (ZIP Notifica-PNT en estado "Pendiente",
2026-07-21):** el parser ya funciona sin cambios — `estado_texto="Pendiente"`
no está en `MAPA_RESULTADO`, `resultado` sale `None` correctamente. El XML
`InformeENI.xml` no aporta ningún dato de negocio adicional al PDF (solo
metadatos ENI de gestión documental + firma CAdES) — confirma que "todo pasa
por el PDF". **Pendiente:** actualizar el docstring de
`app/services/parser_justificante_notifica.py` (todavía dice "sin confirmar"
para estados fuera de "Leída") — deferido a cuando se implemente #657, anotado
en el checklist del issue, no hacerlo antes.

Issues #657 (UI, `NotificarEditor` + hook `editar_tarea`) y #658 (schema +
cotejo) — milestone M2, mismo PR. Ver checklists actualizados en GitHub para
el detalle de implementación.
