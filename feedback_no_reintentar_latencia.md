---
name: feedback-no-reintentar-latencia
description: "Ante tool_results que tardan o no llegan, esperar un solo resultado; NO reemitir el mismo comando en bucle."
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 561effda-ed54-43bb-b8be-ed25942f9768
---

Cuando el harness tarda en devolver el resultado de una herramienta (latencia alta, entrega en bloque/batched), NO reemitir el mismo comando una y otra vez para "forzar el flush". Cada reemisión se ejecuta de verdad y satura: en una sesión llegué a lanzar ~59 `git diff`/`git log` idénticos para ver el estado una sola vez.

**Why:** reemitir comandos idénticos no acelera la entrega de resultados (el problema es la latencia de entrega, no que el comando no haya salido); solo encola ejecuciones reales y desperdicia tiempo y recursos del usuario.

**How to apply:** lanzar el comando UNA vez y esperar su resultado, aunque tarde. Si no llega, esperar — no duplicar. Para comandos de solo lectura (estado git, lecturas), basta un único intento. Reservar el reintento para fallos reales (error devuelto), no para silencio por latencia.
