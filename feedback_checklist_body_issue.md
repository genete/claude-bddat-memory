---
name: feedback-checklist-body-issue
description: "Tareas/checklists pendientes van al cuerpo del issue, no a comentarios"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 7e17b394-34d0-4e2c-aff6-24a487922a82
---

Cuando haya que dejar tareas pendientes o checklists en un issue de GitHub (actualizaciones documentales diferidas, sub-tareas de implementación, etc.), ponerlos en el **cuerpo principal** del issue, no como comentario.

**Why:** Carlos revisa los issues por su cuerpo; los comentarios se le pasan por alto al revisar. Además, los checkboxes del body cuentan para la barra de progreso del issue; los de comentarios no.

**How to apply:** Editar el body con `gh issue edit --body-file` añadiendo la sección de checklist. Si ya se puso como comentario, mover al body y borrar el comentario (`gh api -X DELETE repos/:owner/:repo/issues/comments/<id>`). Relacionado con [[feedback_commit_format]].
