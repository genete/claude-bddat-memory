---
name: project-plantilla-tarea-elaborar
description: Arquitectura de plantillas docx y su relación con tareas ELABORAR — decisión pendiente sobre asociación formal
metadata: 
  node_type: memory
  type: project
  originSessionId: 90cef23a-824c-4dc0-ac3a-9ceefcc95e0b
---

El `.docx` de plantilla es independiente del tipo de documento producido y del CB usado.
La asociación entre qué plantilla puede usarse en cada tarea ELABORAR concreta no está implementada.

El sistema ya tiene una validación previa: antes de renderizar, comprueba que los campos usados en la plantilla existan en el contexto del CB; si faltan, avisa.

**Why:** Mantener ese vínculo plantilla↔tarea-ELABORAR puede ser innecesario si la validación de campos es suficiente garantía de uso correcto.

**How to apply:** No implementar esa asociación salvo decisión explícita del usuario. Cuando se pruebe con plantilla dummy (tras completar CBs #392-#394), evaluar si merece la pena.
