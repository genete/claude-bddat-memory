---
name: feedback_pr_refs_vs_closes
description: "En PRs de issues que tienen múltiples fases, usar Refs"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: a8ec1c1b-f778-4e8f-96f7-e9ed1c718382
---

En issues que se implementan en varias fases (como #500 con S3b-0…S3b-4), los PR bodies deben usar **`Refs #N`**, nunca `Closes #N`, hasta que se llegue al PR de la fase final.

**Why:** El PR de S3b-3 usó `Closes #500` cuando el plan general decía explícitamente "NO Closes — #500 sigue abierto hasta la fase final (eliminar las 5 vistas BC + reconectar enlaces)". El issue se cerró prematuramente y hubo que reabrirlo.

**How to apply:** Antes de redactar cualquier PR body, verificar si el issue sigue abierto a propósito. Si el plan del issue dice que quedan fases o el issue es "paraguas" de varias funcionalidades, usar `Refs #N`. Solo usar `Closes #N` en el PR de cierre definitivo.
