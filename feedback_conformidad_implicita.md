---
name: No asumir conformidad implícita en decisiones de diseño
description: Cuando Claude propone un cambio de diseño y el usuario no responde explícitamente, no asumir que está de acuerdo antes de implementarlo
type: feedback
originSessionId: 202a73f4-af2e-4ca1-a71a-75769460c763
---
No asumir conformidad implícita cuando Claude propone cambios de diseño (nombres de tipos, estructura de trámites, decisiones de modelado, etc.) y el usuario continúa la conversación sin rechazarlo explícitamente.

**Why:** En una sesión de catalogación (#337), Claude sugirió mantener el nombre `ANUNCIO_TABLON` y lo usó directamente en el catálogo. El usuario confirmó que la fuente de verdad decía `TABLON_AYUNTAMIENTOS` y tuvo que corregirse. La conformidad del usuario debe ser explícita antes de implementar una sugerencia de diseño.

**How to apply:** Cuando Claude propone un cambio de nombre, estructura o decisión de diseño, esperar confirmación explícita del usuario antes de escribirlo en el catálogo, el FTT o cualquier fichero de fuente de verdad. Si la conversación avanza sin confirmación, preguntar antes de actuar.
