---
name: feedback-plantilla-dummy-cb
description: No crear plantilla .docx dummy para Context Builders — no tiene uso real
metadata: 
  node_type: memory
  type: feedback
  originSessionId: af530b78-2990-4d67-87ec-a971310e8d61
---

No crear plantilla .docx dummy al implementar un Context Builder.

**Why:** Las plantillas dummy no se usan en desarrollo ni en producción. Se creó una en un CB anterior y fue eliminada inmediatamente. Los tests del CB no usan la plantilla — usan mocks directamente.

**How to apply:** Al implementar un CB (cualquier issue de tipo CB ContextoX), omitir la plantilla .docx del alcance. El seed en `plantillas` sí se registra (apunta a una ruta), pero el fichero físico no se crea hasta que el Supervisor prepare la plantilla real.
