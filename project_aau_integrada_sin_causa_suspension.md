---
name: project-aau-integrada-sin-causa-suspension
description: "El bloque ambiental AAU_AAUS_INTEGRADA no suspende el plazo de la solicitud: desde #778 eso es una fila que falta en catalogo_plazos, no una lista que ampliar en el código"
metadata: 
  node_type: memory
  type: project
  originSessionId: 62cdf2f2-9519-4ffe-8917-9a2a671788e4
  modified: 2026-08-21T06:23:15.577Z
---

Detectado 2026-08-19, sesión #789 (plazo indefinido), a raíz de una consulta de
Carlos a Gemini sobre suspensión de plazos en tramitación de AT
(https://share.gemini.google/cJ1KzPmFYM8e).

**Ningún trámite de `AAU_AAUS_INTEGRADA` suspende el plazo de la solicitud**, pese
a que la espera del dictamen ambiental / propuesta de informe vinculante /
informe vinculante definitivo es estructuralmente idéntica a
`SOLICITUD_COMPATIBILIDAD` (informe preceptivo y determinante de otro órgano).

**Confirmado por Gemini (jurídicamente, no verificado contra BOE/legalize
todavía — pendiente si se retoma):** la práctica correcta es una única
suspensión "por bloque ambiental": se suspende al remitir a Medio Ambiente
(`REMISION_RESULTADO_IP_CONSULTAS`) y se levanta cuando llega el informe
vinculante definitivo (`RECEPCION_INFORME_VINCULANTE`) — sin re-suspender en
cada sub-hito (dictamen, propuesta).

**Qué cambió con #778 (2026-08-21).** Ya no hay `_TRAMITES_SUSPENSION` ni
`_TRAMITES_CIERRE` en `plazos.py`: qué suspende es la columna
`catalogo_plazos.suspende_plazo_solicitud`, y el rescate por trámite hermano
desapareció porque cada plazo se cierra con el documento producido de su propia
espera. Así que esto ya no es «un hueco en una lista del código» sino **poblado
de catálogo**: dar de alta la fila del `ESPERAR_PLAZO` que corresponda, con su
plazo y su marca de suspensión. Sin fila no suspende, y eso es el comportamiento
buscado, no un defecto. Igual pasa con `SOLICITUD_COMPATIBILIDAD`, que dejó de
suspender en #778 por no tener fila — antes estaba en la lista del código sin
entrada en el catálogo, contradicción que ya no puede darse.

El tope de 3 meses del art. 22.1.d tampoco se implementa en el cómputo: la
suspensión acaba, como mucho, al vencer el plazo del catálogo (lectura corta), y
un valor mayor de tres meses solo produce un aviso al dar de alta la entrada.

**Sigue sin verificarse** si `SOLICITUD_FIGURA` (`FIGURA_AMBIENTAL_EXTERNA`)
merece el mismo tratamiento. Ver [[project_bloqueos_naturales_vs_motor]].
