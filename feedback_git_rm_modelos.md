---
name: feedback-git-rm-modelos
description: "Al eliminar ficheros de modelo del proyecto, usar git rm en el mismo issue — no dejarlos como código muerto"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 8946eb33-7ef8-45f0-9a17-c18bd095518c
---

Cuando el alcance de un issue incluye eliminar ficheros (modelos, servicios, etc.), ejecutar `git rm` como parte de la implementación, no como paso posterior.

**Why:** Dejar ficheros sin importar en el repo es incompleto: siguen apareciendo en `git ls-files`, contaminan búsquedas y dan sensación de deuda técnica pendiente. Si el issue dice "eliminar", `git rm` es parte del trabajo, no un extra.

**How to apply:** En el commit de modelo/servicio correspondiente, incluir los `git rm` de los ficheros obsoletos junto con los cambios de código. No esperar a que el usuario lo señale.
