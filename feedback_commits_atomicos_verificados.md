---
name: feedback_commits_atomicos_verificados
description: "En refactors grandes, trocear en commits atómicos verificados uno a uno en vez de un commit monolítico"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 86c11189-37b7-471f-982f-689dcf6bf0aa
---

En refactors que tocan muchos ficheros (p. ej. #533 ADR-022 sistema visual base), trocear en cambios atómicos que dejen la navegación coherente y verificar cada uno (visualmente / con tests) antes de pasar al siguiente. No acumular todo en un único commit grande.

**Why:** un commit monolítico de muchos ficheros es difícil de depurar si algo se descuadra; los cambios atómicos aíslan la causa.
**How to apply:** secuencia tipo "tipografía → comprobar → commit; tokens de color → comprobar → commit; tabla unificada → comprobar → commit; retirar recorte → comprobar → commit; docs → commit". Relacionado con [[feedback_analisis_impacto]] y [[feedback_una_conversacion_por_issue]].
