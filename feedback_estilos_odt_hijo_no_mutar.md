---
name: feedback-estilos-odt-hijo-no-mutar
description: "En estilos ODT/LibreOffice, añadir una variante (p.ej. mayúsculas) como estilo HIJO que solo aporta esa propiedad, nunca mutando el estilo compartido existente"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 5772ee62-b715-411b-abfa-f0e5bbf52ab9
  modified: 2026-08-05T07:07:44.004Z
---

Cuando un estilo de párrafo ODT existente necesita una variante puntual (p.ej. "el mismo título pero en mayúsculas"), crear un estilo nuevo con `style:parent-style-name` apuntando al original, que solo declare la propiedad añadida (`fo:text-transform="uppercase"` y nada más). Nunca añadir la propiedad directamente al estilo compartido.

**Why:** En #728 (ADR-039 §5, estilo `Cabecera - Delegación Territorial` para el título de la resolución) primero muté `BDDAT - Título resolución` añadiéndole `fo:text-transform="uppercase"` in-place. Carlos lo corrigió: al ser hijo en vez de mutación, (1) el estilo compartido queda limpio y reutilizable en texto normal si algún día hace falta, (2) cualquier cambio futuro en el padre (tipo, tamaño, espaciado) lo hereda el hijo automáticamente sin tener que tocarlo, y (3) el uppercase queda aislado y fácil de quitar sin arriesgar el resto del formato.

**Segundo error relacionado, mismo issue:** interpreté mal ADR-039 §5 y apliqué el uppercase a la segunda línea del MEMBRETE de la carta (`carta_base.odt`), cuando el ADR se refería al ENCABEZAMIENTO/TÍTULO de la resolución (`fabricar_plantilla_resolucion.py`). El membrete debía quedar intacto, idéntico al origen JdA. Antes de tocar una plantilla ODT compartida entre carta y resolución, confirmar en qué fichero/plantilla vive realmente el texto que el ADR describe — "cabecera"/"encabezamiento" es ambiguo entre el membrete (letterhead, familia de estilos `Cabecera - *`) y el título del cuerpo de un documento tipo resolución.

**How to apply:** Al implementar cualquier estilo ODT nuevo derivado de uno existente, usar `style:parent-style-name` + solo la(s) propiedad(es) que cambian. Ver [[feedback_conformidad_implicita]] — en plantillas legales/oficiales (resoluciones), confirmar el emplazamiento exacto antes de generar los .odt reales, no asumir por el nombre del ADR.
