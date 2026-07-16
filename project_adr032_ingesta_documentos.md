---
name: project-adr032-ingesta-documentos
description: ADR-032 — dos mecanismos de entrada al pool (registrar in situ vs. multipart), rutas relativas, encaje por vinculación; origen de una regresión real detectada en #180
metadata:
  node_type: memory
  type: project
  originSessionId: 994c1951-3488-4b80-82ec-a5d55264839a
---

`docs/decisiones/ADR-032-ingesta-almacenamiento-fisico-documentos.md` (adoptada
2026-07-16, commit directo a develop) fija:

- **Dos conceptos de entrada al pool**, donde antes (#180) solo existía uno: "registrar in
  situ" (documento ya en su ubicación definitiva bajo `FILESYSTEM_BASE`, solo se localiza
  su URL vía explorador ad-hoc, sin copiar) y "subir al pool" (documento en cualquier otro
  sitio — típicamente el PC del usuario — vía diálogo nativo del navegador/multipart,
  siempre copia a `AT-N/pool/<hash-sha256>_<nombre>`). Tras la entrada, las operaciones
  posteriores son idénticas sea cual sea el origen.
- `Documento.url` (esquema local) debe ser siempre relativa a `FILESYSTEM_BASE` — corrige
  una regresión real, no introduce diseño nuevo (ver más abajo).
- Encaje: el documento se mueve a su carpeta ESFTT legible (códigos inmutables de
  catálogo: `tipos_fases.codigo`/`tipos_tramites.codigo`/`tipos_tareas.codigo`) solo en su
  **primera** vinculación a tarea (`DocumentoTarea`, ADR-010); vinculaciones posteriores
  (multi-consumo) no repiten el movimiento.

**Hallazgo de regresión (relevante si se audita `Documento.url` en el futuro):** el commit
`3a57a8a` (#180, 2026-03-07 17:19) guardaba ruta relativa (`url_relativa`); el commit
`a41c80b`, tres horas después el mismo día, sustituyó el mecanismo de subida por el
explorador ad-hoc del servidor y cambió a ruta absoluta (`ruta_abs`) como efecto colateral
no deliberado — nunca se corrigió hasta esta sesión. Confirmado además contra BD real de
desarrollo: de 88 filas `documentos.url`, solo 1 era absoluta (y esa venía del generador de
escritos, no del pool), 27 relativas de `scripts/seed_demo.py` (dataset ficticio nunca
conectado al código real). Ver [[feedback_verificar_con_datos_reales_e_historial]].

Issues del bloque: #664 (rutas relativas) → #665 (pool + convención carpetas) →
{#666 (multipart), #667 (mover al vincular)}. Ver también
[[project_organizacion_documental_pendiente]].
