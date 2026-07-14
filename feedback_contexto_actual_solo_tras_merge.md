---
name: feedback_contexto_actual_solo_tras_merge
description: "CONTEXTO_ACTUAL.md se actualiza solo inmediatamente después de mergear el PR, y \"Hecho\" no se encola — solo lo ÚLTIMO hecho"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 367fd960-d053-4b89-b23d-7dc766be57f1
---

`docs/CONTEXTO_ACTUAL.md` — sección "Hecho": actualizar **inmediatamente después
de mergear** el PR que cierra el issue, nunca antes (mientras el PR sigue
`OPEN` no se marca como Hecho, aunque el trabajo esté terminado y verificado).

No encolar histórico en "Hecho": anotar sucintamente solo lo ÚLTIMO hecho,
podando lo anterior (el historial completo vive en `git log`). Coherente con
el propio commit `bbbc418 [DOCS] CONTEXTO_ACTUAL: podar Hecho a lo último,
historial vive en git log`.

**Por qué:** Carlos corrigió explícitamente tras pedir actualizar
`CONTEXTO_ACTUAL.md` con #632 marcado Hecho mientras el PR #634 seguía
abierto sin mergear — el documento debe reflejar estado real de `develop`,
no trabajo en rama/PR pendiente.

**Cómo aplicarlo:** Antes de tocar la sección "Hecho", comprobar con
`gh pr view <N> --json state,mergedAt` que el PR está mergeado. Si sigue
`OPEN`, no editar el documento — esperar confirmación o a que el merge
ocurra. Al editar, sustituir el contenido de "Hecho" por el issue recién
cerrado (no añadir a una lista creciente). La sección "Próximo" sigue
requiriendo propuesta + confirmación explícita del usuario (regla ya
existente en `CLAUDE.md`), independiente de esta.
