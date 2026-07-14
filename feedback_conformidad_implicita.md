---
name: No asumir conformidad implícita en decisiones de diseño
description: Cuando Claude propone un cambio de diseño y el usuario no responde explícitamente, no asumir que está de acuerdo antes de implementarlo
type: feedback
originSessionId: 202a73f4-af2e-4ca1-a71a-75769460c763
---
No asumir conformidad implícita cuando Claude propone cambios de diseño (nombres de tipos, estructura de trámites, decisiones de modelado, etc.) y el usuario continúa la conversación sin rechazarlo explícitamente. Incluye también el caso de **reescribir una sección de un documento fuente de verdad** (p. ej. "Próximos" de `docs/CONTEXTO_ACTUAL.md"): no basta con no rechazar contenido nuevo — tampoco vale **eliminar contenido existente** por inferencia propia de que "ya no hace falta" sin confirmarlo.

**Why:** En una sesión de catalogación (#337), Claude sugirió mantener el nombre `ANUNCIO_TABLON` y lo usó directamente en el catálogo. El usuario confirmó que la fuente de verdad decía `TABLON_AYUNTAMIENTOS` y tuvo que corregirse. Después (cierre de #171), Claude reescribió "Próximos" de `CONTEXTO_ACTUAL.md` para señalar el siguiente issue y, de paso, borró sin preguntar la mención del spinoff #612 (N034) razonando que "ya es un issue normal del backlog" — pero Carlos lo consideraba una tarea que quería hacer, y además `CLAUDE.md` ya exige explícitamente pedir confirmación antes de tocar ese fichero. La conformidad del usuario debe ser explícita antes de implementar una sugerencia de diseño **o de quitar algo que ya estaba**.

**How to apply:** Cuando Claude propone un cambio de nombre, estructura o decisión de diseño, o cuando reescribe una sección de `CONTEXTO_ACTUAL.md` u otra fuente de verdad, esperar confirmación explícita del usuario antes de escribirlo — y si la reescritura implica eliminar algo que ya figuraba, señalarlo explícitamente en la propuesta en vez de dejarlo caer en silencio. Si la conversación avanza sin confirmación, preguntar antes de actuar.
