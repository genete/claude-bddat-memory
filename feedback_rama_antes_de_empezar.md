---
name: feedback_rama_antes_de_empezar
description: "Crear la rama feature/ ANTES de hacer cualquier edición, no después de commitear en develop"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 5817fdd4-455c-4129-a96f-d74699117584
  modified: 2026-08-05T06:38:31.947Z
---

Crear la rama y cambiar a ella ANTES de escribir la primera línea de código. No al final.

**Why:** En esta sesión se hicieron 15 ficheros de cambio directamente en develop y se commiteó ahí. Fue necesario un cherry-pick + reset para rectificarlo — trabajo extra y riesgo de pérdida de contexto.

**How to apply:** En cualquier tarea que toque 3+ ficheros, modelos, rutas o templates (ver REGLAS_DESARROLLO.md §"Commit directo vs rama temporal"), el primer comando es siempre:
`git -C /d/BDDAT checkout -b feature/issue-NN-descripcion`
Solo después se editan ficheros. No hay excepción aunque el cambio "parezca sencillo".

**Recaída (sesión 2026-07-23, batería #700/#693/#697/#694):** el fallo se repitió dos veces
en la misma sesión — se editó `AnalizarEditor.jsx` en `develop` antes de crear la rama, tanto
para #700 como para #694 (se corrigió a tiempo con `checkout -b` sobre el working tree sin
commitear, sin necesitar cherry-pick). Patrón detectado: ocurre específicamente al pasar
directo de "leer el diagnóstico del issue con `gh issue view`" a "aplicar el `Edit`" sin un
paso intermedio explícito de `git checkout -b`. **Anteponer siempre** la creación de rama
inmediatamente después de leer el issue y antes de la primera tool call de `Edit`/`Write`
sobre código — tratarlo como un paso obligatorio de la lista, no como algo a recordar por
contexto.

**Recaída (sesión 2026-08-05, #728 fase 2 — CRUD tras mergear el modelo):** mismo issue,
misma conversación, segunda fase de trabajo: tras mergear el PR de las migraciones (rama
borrada, vuelta a `develop`), el usuario pidió continuar con la exposición Capa 1 + CRUD.
Se encadenó un Explore de investigación de patrones (varias tool calls de research) seguido
directamente de `Write`/`Edit` sobre código, sin volver a crear rama — la vuelta a `develop`
tras el merge anterior "resetea" la guardia mental de la regla. Se corrigió a tiempo
(`checkout -b` con el working tree sucio, sin commitear en `develop`) al llegar al primer
`git status` antes de commitear. **Ampliación de la regla:** el paso de `git checkout -b` va
inmediatamente después de CUALQUIER momento en que la rama activa es `develop`/`main` y se
va a escribir código — no solo al principio de una conversación nueva, también al reanudar
un issue tras cerrar/mergear su fase anterior en la misma sesión.
