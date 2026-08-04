---
name: feedback_umbral_factorizar_excepciones_bespoke
description: "Cuándo factorizar una excepción de un editor bespoke de tarea (ANALIZAR/ELABORAR/NOTIFICAR) en un patrón compartido vs dejarla como caso único — criterio de raíz común, no solo recuento de apariciones"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: ec260c83-0a5f-4ab4-a4bf-1a7bd31cfe22
  modified: 2026-08-04T05:42:26.322Z
---

En #742 (2026-08-04) extraje el botón de borrado como componente propio (`BotonBorrarTarea`
en `Inspector.jsx`) porque aparecía idéntico en los tres editores bespoke de tarea
(ANALIZAR/ELABORAR/NOTIFICAR) — caso limpio de "Rule of Three" con forma idéntica. Carlos
preguntó por qué, con ese mismo criterio, no se extrae también el ocultamiento de la
Despensa (`ocultarDespensa`/`deshabilitarProducido`, hoy solo en ANALIZAR).

**Por qué ese no (todavía):** el ocultamiento de Despensa es una única instancia — solo
ANALIZAR la oculta, por una razón propia suya (`analizarSeccionesExtendidas`: casar un
requisito documental deriva el consumido, la Despensa deja de tener uso). No es un patrón
repetido 3 veces de forma idéntica como el botón de borrar; factorizarlo ahora sería
generalizar a partir de N=1 — abstracción prematura.

**El criterio que sí importa, más allá de contar apariciones:** si una excepción de "editor
bespoke oculta/condiciona algo del contenedor común (`InspectorEdicion`)" nace de la MISMA
causa raíz en un segundo sitio — aquí, "el editor gestiona su propio vínculo de documento y
por eso el mecanismo genérico deja de tener sentido" —, eso ya no es un caso único: es un
patrón con 2 raíces iguales y toca proponer una interfaz (p. ej. cada editor bespoke declara
si gestiona su propio vínculo documental) en vez de seguir apilando condicionales
`esAnalizar`/`esX` en `InspectorEdicion`.

**Cómo aplicar:** al tocar `Inspector.jsx` o los editores bespoke en trabajo futuro sobre
fases con más profundidad de diseño (CONSULTAS, INFORMACION_PUBLICA — ver
[[project_diseno_tarea_analizar_442]] para el reparto inspector/modal ya acordado), si
aparece una SEGUNDA excepción de este tipo con la misma raíz que `ocultarDespensa`,
señalarlo a Carlos como candidato a factorizar en cuanto se detecte — no esperar a una
tercera aparición ni copiar el condicional sin más.
