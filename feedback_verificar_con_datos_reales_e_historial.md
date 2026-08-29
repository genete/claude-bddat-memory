---
name: feedback-verificar-con-datos-reales-e-historial
description: "Ante duda \"esto no me cuadra\" sobre código, verificar con git log/show y datos de BD — pero la BD de desarrollo ilustra, no prueba: es desechable, ficticia y con residuos previos a las salvaguardas"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 7348668e-566e-4672-8d65-967da640fa0a
  modified: 2026-08-29T07:49:44.224Z
---

Cuando Carlos cuestiona una afirmación sobre el comportamiento del código ("esto no me
cuadra", "no recuerdo que fuera así", "puede que sea una regresión"), no basta con releer
el código actual para confirmar o desmentir — eso solo confirma el estado presente, no si
es diseño original o desviación accidental.

**Why:** En la sesión de organización documental (2026-07-16), Carlos dudó de que
`Documento.url` se guardara como ruta absoluta ("me chirría desde el inicio"). `git log -p`
sobre `app/modules/expedientes/routes.py` reveló que el commit `3a57a8a` (#180) guardaba
ruta relativa y que `a41c80b`, tres horas después el mismo día, la cambió a absoluta como
efecto colateral no deliberado de sustituir el mecanismo de subida — una regresión real,
no una decisión de diseño. Contrastar además con la BD de desarrollo abarató mucho el
alcance de la corrección. Sin ambas verificaciones (historial + datos), el issue se habría
encuadrado como "cambio de diseño" en vez de "corrección de regresión con migración casi
nula".

**How to apply:** ante ese tipo de duda, encadenar `git log -p --follow <fichero>` (o
`git blame`) para ver la evolución real, y si hay BD accesible (MCP postgres), consultar
los datos antes de responder.

## Qué valor probatorio tiene la BD de desarrollo (matiz de 2026-07-29)

**La BD de desarrollo ilustra y abarata; no prueba.** Carlos lo señaló al revisar el
análisis de #711, donde deduje "este patrón es letra muerta" de un recuento de filas:

- Hay valores **creados antes de las salvaguardas actuales**, o directamente sin reglas.
  Una fila inconsistente puede ser residuo histórico, no evidencia de un bug vivo.
- Los datos operacionales son **ficticios y desechables**: sin valor legal, administrativo
  ni de ningún otro tipo.

Consecuencia para las afirmaciones: la ausencia de un dato NO demuestra que el sistema no
pueda producirlo. Antes de escribir "esto no ocurre nunca" en un issue o un ADR, buscar el
soporte en el **código** (¿qué caminos escriben ese dato?) y usar el recuento de BD solo
como indicio coherente, diciéndolo como indicio. Formulación correcta: *"ningún camino
automático lo crea; solo un gesto manual no guiado — y en la BD de desarrollo no aparece
ninguna vez"*, en vez de *"no existe jamás"*.

## No revertir datos tras pruebas de interfaz

Al verificar en navegador, **no deshacer los cambios provocados** (borrar filas de alta,
restaurar estados previos, limpiar). Cuesta tokens y turnos, y para Carlos corregir o
borrar esos datos es inmediato y gratuito. Dejarlos y decir qué se ha tocado.

Nota: esto NO afecta a los **tests** — los smoke tests corren contra BD real y ahí sí hace
falta fixture `autouse` que limpie (ver [[feedback_no_worktrees_bddat]]). La diferencia es
que un test sucio rompe la siguiente ejecución; una fila de prueba en la UI, no.

Relacionado: [[feedback_verificar_atribucion_codigo]], [[feedback_ficheros_temp]].
