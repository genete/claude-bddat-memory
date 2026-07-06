---
name: feedback-emplazamiento-navegacion
description: "Antes de dar de alta una pantalla de administración nueva, decidir su emplazamiento de navegación con el criterio de ADR-029, no copiar el módulo hermano más parecido"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: d68a763c-d4d2-48f9-a5d8-c81e34af0242
---

Al construir una pantalla de administración nueva (CRUD, panel de configuración...), el
patrón de UI a reutilizar (listado+inspector, ADR-023) y el emplazamiento de navegación
(entrada propia de sidebar vs. tarjeta dentro del hub del supervisor) son decisiones
**distintas** — no vienen juntas por copiar el módulo hermano más parecido en el código.

**Por qué:** en #583 (CRUD de `requisitos_documentales`) se copió correctamente el patrón de
UI de `admin_plantillas`, pero también su emplazamiento (`/admin/*`, módulo propio con
sidebar propio) sin releer ADR-028, que ya había fijado que ese tipo de CRUD vive como
tarjeta del hub del supervisor. El error no se detectó al escribir el código ni en su
revisión — lo encontró el usuario en la sesión siguiente, al ver la entrada de sidebar sin
enlace desde el hub. Resuelto y generalizado en ADR-029
(`docs/decisiones/ADR-029-navegacion-administrativa.md`).

**Cómo aplicar:** antes de elegir dónde vive una pantalla nueva, aplicar el test de ADR-029
§1 (¿la consultan a diario roles no-supervisores, o es configuración pura del supervisor?) —
no asumir que el módulo más parecido ya acertó su propio emplazamiento; puede ser legacy
anterior a la decisión de navegación vigente. `REGLAS_DESARROLLO.md` §Control de acceso
enlaza a este criterio.
