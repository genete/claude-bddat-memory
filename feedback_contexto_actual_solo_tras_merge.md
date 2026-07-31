---
name: feedback_contexto_actual_solo_tras_merge
description: "CONTEXTO_ACTUAL.md se actualiza solo tras mergear el PR, \"Hecho\" no se encola (solo lo ÚLTIMO), y registra únicamente lo que NO está en ADRs, MDs ni issues"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 367fd960-d053-4b89-b23d-7dc766be57f1
  modified: 2026-07-30T11:49:57.453Z
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

## Qué contenido va (2026-07-30)

El documento **registra**, no describe. Quien lo lea va a abrir de todas formas
los ADRs, los documentos de diseño y los issues que se citen, así que resumir
su contenido aquí solo alarga. En "Hecho" va **únicamente lo que no está
escrito en ninguno de ellos** — típicamente una regla de método o un supuesto
que no tiene otro sitio donde vivir — más un puntero a dónde está el detalle.

En "Próximo": la lista de issues **por su título, sin el porqué extendido**.
Ni "#N porque depende de #M": si la dependencia importa, está en el issue.
Como mucho, "siguiente #nnn" y luego los demás.

Lo que sí merece quedarse porque no vive en otro sitio por definición: los
**huecos de diseño sin issue**, las **ausencias deliberadas** ("sin issue a
propósito", para que nadie lo abra creyendo que es un olvido) y el motivo en
tres palabras de cada aplazamiento (el issue no dice por qué está aparcado).

**Por qué:** Carlos pidió podar un "Hecho" de veinte líneas que repetía lo que
ya decían ADR-035, el documento de diseño y los comentarios del issue. Su
frase: es un sitio para registrar, no descriptivo.

**Patrón de fallo concreto (recurrente, 2026-07-16):** el reflejo natural al
cerrar un issue nuevo es *añadir* su resumen a continuación del texto de
Hecho ya existente ("por si acaso se pierde contexto del cierre anterior").
Eso es exactamente el error — Carlos tuvo que pedir explícitamente "aligera
Hecho, solo lo último" tras verlo. Antes de escribir el Edit, releer la regla:
el nuevo contenido de Hecho **reemplaza por completo** al anterior, no lo
precede ni lo sigue.
