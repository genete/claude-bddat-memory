---
name: feedback_browser_pane_no_mostrado
description: "computer{action:screenshot} puede fallar con 'Browser pane is not displayed' — depende de que el panel esté visible en pantalla del usuario, no de la isla React que se esté probando"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: ec260c83-0a5f-4ab4-a4bf-1a7bd31cfe22
  modified: 2026-08-04T05:28:13.824Z
---

En sesión #742 (2026-08-04), `mcp__Claude_Browser__computer{action:"screenshot"}` falló
repetidamente con «Screenshot timed out... the Browser pane is not displayed, so the page
is not compositing frames» al verificar el árbol de expediente (react-flow, mismo tipo de
vista que [[project_verif_arbol_react]]). Reintentar (wait + screenshot) no lo arregló.

**Por qué:** la compositación de frames para `screenshot`/clicks por `coordinate` exige que
el panel del navegador integrado esté **visualmente mostrado** en la interfaz de Claude Code
Desktop — es un estado de la sesión/cliente, no un límite de la isla React. Prueba: la
misma vista (árbol react-flow, incluso el mismo expediente AT-2004/id=4) funcionó con
screenshot sin problema en la sesión #701 (ver [[project_verif_arbol_react]]) — el problema
no es react-flow. No existe ninguna tool de Claude para forzar que el panel se muestre; es
una acción del lado del usuario en su interfaz (encaja con lo que Carlos describe: en otras
sesiones "dar a abrir" y luego redimensionar antes de que las coordenadas funcionaran).

**Cómo aplicar:**
- Si `screenshot` falla así, no reintentar en bucle ni pelear con `find`/`read_page` para
  sacar `ref_N` de nodos custom sin rol ARIA (los nodos `.arbol-tarea` de react-flow son
  `<div>` con handler React, no elementos accesibles — `read_page filter:interactive` no
  los etiqueta).
- Si basta con confirmar estructura/estado/comportamiento (no aspecto visual real), ir
  directo a `javascript_tool` (dispatchEvent de `mousedown`/`mouseup`/`click` sobre el nodo
  vía `querySelector`) + `get_page_text` o lectura de DOM (`innerText`, atributos) para leer
  el resultado. Es más barato en contexto que pelear por una captura y funciona igual de
  bien para verificar wiring/lógica.
- Si hace falta de verdad una captura visual (CSS, layout, algo que no se pueda confirmar
  por texto/DOM), decírselo a Carlos y pedirle que muestre/abra el panel del navegador
  integrado en su pantalla antes de reintentar — no asumir que un `wait` lo resuelve solo.
