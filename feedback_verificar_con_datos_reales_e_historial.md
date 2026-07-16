---
name: feedback-verificar-con-datos-reales-e-historial
description: "Ante una duda del usuario de tipo \"esto no me cuadra\" sobre comportamiento de código, verificar con git log/show y datos reales de BD, no solo releer el código actual"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 7348668e-566e-4672-8d65-967da640fa0a
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
no una decisión de diseño. Contrastar además con `psql` contra la BD de desarrollo confirmó
que casi ningún dato real reflejaba el "diseño" que el código actual sugería (solo 1 de 88
filas), lo que además abarató mucho el alcance de la corrección. Sin ambas verificaciones
(historial + datos reales), el issue se habría encuadrado como "cambio de diseño" en vez de
"corrección de regresión con migración casi nula" — cambia la prioridad y el coste
percibido.

**How to apply:** ante ese tipo de duda, encadenar `git log -p --follow <fichero>` (o
`git blame`) para ver la evolución real, y si hay BD accesible (MCP postgres o `psql`
directo — ver `.env` para `DATABASE_URL`), consultar los datos reales antes de responder.
Aplica sobre todo en sesiones de definición/arquitectura, donde una hipótesis errónea sobre
"por qué está así" cambia el encuadre de todo un bloque de issues.
