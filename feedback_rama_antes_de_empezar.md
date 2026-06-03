---
name: feedback_rama_antes_de_empezar
description: "Crear la rama feature/ ANTES de hacer cualquier edición, no después de commitear en develop"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 5817fdd4-455c-4129-a96f-d74699117584
---

Crear la rama y cambiar a ella ANTES de escribir la primera línea de código. No al final.

**Why:** En esta sesión se hicieron 15 ficheros de cambio directamente en develop y se commiteó ahí. Fue necesario un cherry-pick + reset para rectificarlo — trabajo extra y riesgo de pérdida de contexto.

**How to apply:** En cualquier tarea que toque 3+ ficheros, modelos, rutas o templates (ver REGLAS_DESARROLLO.md §"Commit directo vs rama temporal"), el primer comando es siempre:
`git -C /d/BDDAT checkout -b feature/issue-NN-descripcion`
Solo después se editan ficheros. No hay excepción aunque el cambio "parezca sencillo".
