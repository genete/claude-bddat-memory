---
name: feedback_relectura_contexto
description: No releer ficheros que ya están en el contexto de la conversación actual
type: feedback
originSessionId: 792b5b83-34e1-4b58-868e-e670fb235eae
---
No usar Read sobre un fichero que ya fue leído en la misma sesión si su contenido sigue visible en el contexto. Es un desperdicio de tokens y tiempo.

**Why:** El usuario lo señaló explícitamente al ver que REGLAS_DESARROLLO.md fue leído dos veces en la misma conversación con apenas unos turnos de diferencia.

**How to apply:** Antes de llamar a Read, comprobar si el fichero ya fue leído en esta sesión y su contenido sigue en contexto. Si es así, trabajar directamente desde ese contenido.
