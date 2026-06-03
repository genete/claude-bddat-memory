---
name: project_verif_arbol_react
description: "Cómo verificar la vista de árbol React (expediente-arbol/react-flow) en navegador; preview_screenshot se cuelga, usar Playwright MCP"
metadata: 
  node_type: memory
  type: project
  originSessionId: 0b057c39-2c1e-4162-9252-46aa9b4caade
---

Verificar la isla **expediente-arbol** (react-flow) en navegador tiene varias trampas de tooling:

- **`preview_screenshot` se CUELGA (timeout 30s) en la vista de árbol**: el bucle `requestAnimationFrame`
  continuo de react-flow impide al capturador alcanzar estado "idle". El renderer está vivo
  (`preview_snapshot` y `preview_eval` funcionan siempre). → Para capturas usar **Playwright MCP**
  (navegador VISIBLE): `browser_navigate` + login fetch + `browser_take_screenshot`. Guardar siempre en
  `.playwright-mcp/nombre.png` (si das nombre sin prefijo, cae en la raíz del repo, untracked, y `rm`/`mv`
  están bloqueados).
- **El navegador de preview (`preview_start` "bddat", 5000) es HEADLESS**: el `Screenshot` de windows-mcp
  captura el escritorio real del usuario, no la preview. No sirve para ver la isla.
- **CSSTransition se queda `pending` en `currentTime:0` en el preview headless**: medir `opacity` de un
  atenuado con transición (p.ej. `body.arbol-lock .app-main{opacity:.5;transition:...}`) devuelve el frame
  inicial (1), no el final. Para medir el valor real: anular la transición (`el.style.transition='none'`)
  antes de leer `getComputedStyle`, o medir en el navegador Playwright visible (allí la transición corre).
- **`preview_click` (MCP) no siempre dispara el `onClick` de React**; el click nativo sí: usar
  `preview_eval`/`browser_evaluate` con `elemento.click()`. Para inputs controlados de React, setear el
  valor con el setter nativo del prototipo + `dispatchEvent(new Event('input'/'change',{bubbles:true}))`.

Login de dos pasos por fetch: ver [[project_login_dos_pasos]]. Expediente de prueba habitual: AT-2009
(id=9), rol SUPERVISOR (id=2).
