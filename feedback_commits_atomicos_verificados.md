---
name: feedback_commits_atomicos_verificados
description: "En refactors grandes, trocear en commits atómicos verificados uno a uno en vez de un commit monolítico"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 86c11189-37b7-471f-982f-689dcf6bf0aa
  modified: 2026-08-05T06:38:42.827Z
---

En refactors que tocan muchos ficheros (p. ej. #533 ADR-022 sistema visual base), trocear en cambios atómicos que dejen la navegación coherente y verificar cada uno (visualmente / con tests) antes de pasar al siguiente. No acumular todo en un único commit grande.

**Why:** un commit monolítico de muchos ficheros es difícil de depurar si algo se descuadra; los cambios atómicos aíslan la causa.
**How to apply:** secuencia tipo "tipografía → comprobar → commit; tokens de color → comprobar → commit; tabla unificada → comprobar → commit; retirar recorte → comprobar → commit; docs → commit". Relacionado con [[feedback_analisis_impacto]] y [[feedback_una_conversacion_por_issue]].

**Aplica también a peticiones que agrupan varias piezas en un solo mensaje** (p.ej. #728:
"exposición de campos, CRUD de las nuevas tablas y CRUD de usuarios" en una sola frase) — no
es solo para refactors grandes. Carlos lo remarcó en vivo ("commits atomicos") cuando el plan
por defecto iba a agrupar dos módulos nuevos (organo_propio + firmantes_portafirmas) en un
mismo commit porque compartían ficheros (permisos.py, tarjeta del hub).

**Técnica para partir un hunk compartido entre dos commits** cuando dos piezas añaden bloques
contiguos al mismo fichero (sin línea sin cambios entre ellos, así que `git add -p` no puede
partir el hunk): editar temporalmente el fichero para quitar el segundo bloque, `git add` +
commit de la primera pieza + el fichero compartido recortado; luego un segundo `Edit` que
re-añade el bloque quitado, `git add` + commit de la segunda pieza. Dos ediciones extra, pero
mantiene cada commit revisable y verificado por separado sin recurrir a `git reset --hard` ni
cherry-pick.
