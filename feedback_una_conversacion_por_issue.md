---
name: feedback-una-conversacion-por-issue
description: El usuario prefiere una conversación separada por issue/tarea para poder revisar el historial por nombre de sesión
metadata: 
  node_type: memory
  type: feedback
  originSessionId: b3a72d9e-1f30-48bf-beaf-c985607d82bf
---

Una conversación = un issue o tarea. No continuar con el siguiente issue en la misma sesión aunque haya contexto disponible.

**Why:** El usuario renombra las sesiones por issue (`/rename Implementar #289`) y las revisa después por nombre. Si una sesión contiene varias tareas, el título no refleja el contenido real y la revisión se complica.

**How to apply:** Al cerrar un issue, si el usuario pregunta qué sigue o pide implementar el siguiente, indicar que conviene abrir una conversación nueva. No iniciar trabajo de un issue distinto en la sesión actual.
