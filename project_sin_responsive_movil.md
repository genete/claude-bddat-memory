---
name: project-sin-responsive-movil
description: BDDAT es herramienta de escritorio (Windows) — no se diseña responsive para móvil, solo variación entre resoluciones de escritorio
metadata:
  node_type: memory
  type: project
  originSessionId: 13c005d3-e7b5-4762-9968-110dd1d8e5fb
  modified: 2026-08-29T07:43:54.268Z
---

Decidido 2026-04-15: la app es una herramienta de gestión administrativa — el caso de uso real es siempre escritorio Windows. No hacer responsive para móvil.

**Why:** La experiencia en móvil es estructuralmente mala (cabeceras, footers apilados, listados ilegibles) y el esfuerzo no tiene retorno real.

**How to apply:** No añadir breakpoints ni lógica CSS orientada a móvil. Si se trabaja en responsive, orientarlo solo a variación de resoluciones de monitor de escritorio (no dispositivos móviles). Ignorar viewports < ~1024px.
