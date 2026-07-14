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

**Extensión (mayo 2026):** La regla aplica también cuando se trabaja sobre issues derivados del actual. Actualizar un issue es editar su texto en GitHub; implementarlo (migraciones, código) es trabajo de una sesión nueva. No crear ficheros de implementación aunque parezcan "de paso".

**Excepción explícita (julio 2026, #588/#589/#590):** Cuando varios issues son piezas de la misma ADR/decisión de diseño y comparten contexto de conocimiento profundo (p. ej. los tres issues de reparto de una misma ADR "por unidad de PR"), el usuario puede pedir encadenarlos en una sola sesión — rama+PR por issue igualmente, pero sin abrir conversación nueva entre ellos. El criterio que dio el usuario: "al estar relacionados los tres íntimamente, el contexto se reaprovecha mejor que un hilo para cada uno". No asumir esta excepción por iniciativa propia — pedirla explícitamente si varios issues muy relacionados podrían beneficiarse, pero esperar confirmación antes de encadenar.

