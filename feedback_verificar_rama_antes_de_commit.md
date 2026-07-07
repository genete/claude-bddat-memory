---
name: feedback-verificar-rama-antes-de-commit
description: Verificar la rama git actual antes de cualquier commit en sesiones largas — el snapshot inicial de gitStatus puede quedar obsoleto si el usuario cambia de rama durante la conversación
metadata: 
  node_type: memory
  type: feedback
  originSessionId: a1884c25-d951-4c6c-87a1-7fd95f0777d5
---

En una sesión larga (diseño de ADR-030), estaba a punto de commitear y pushear directo a develop confiando en el snapshot de gitStatus del inicio de la conversación ("Current branch: develop"). Carlos avisó a tiempo: estaba trabajando en paralelo en otra rama (feature/issue-442-contenedor-analizar) para otro asunto. Al comprobar `git status`, en efecto el repo ya no estaba en develop — había cambiado de rama en algún punto de la sesión, fuera de mis tool calls.

**Why:** El snapshot de gitStatus al inicio de la conversación es un punto en el tiempo, no estado en vivo — el propio sistema lo advierte, pero es fácil olvidarlo en sesiones largas donde el usuario puede cambiar de rama por su cuenta (fuera de las tool calls que yo veo) para atender otro trabajo en paralelo.

**How to apply:** Antes de cualquier `git commit`/`git push`, especialmente en sesiones largas o si ha pasado tiempo desde el snapshot inicial, verificar `git status`/rama actual con una llamada real — nunca asumir el estado de la rama por el snapshot inicial de la conversación. Si el resultado no es la rama esperada, crear una rama de seguridad desde `develop` para el commit en vez de tocar la rama activa del usuario, y devolver el repo a la rama original del usuario al terminar.
