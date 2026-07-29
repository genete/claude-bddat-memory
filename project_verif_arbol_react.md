---
name: project_verif_arbol_react
description: "Cómo verificar la vista de árbol React (expediente-arbol/react-flow) en navegador; usar el navegador integrado de Claude Code Desktop (mcp__Claude_Browser__*), no Playwright MCP"
metadata: 
  node_type: memory
  type: project
  originSessionId: 0b057c39-2c1e-4162-9252-46aa9b4caade
  modified: 2026-07-23T06:29:23.986Z
---

**Actualizado 2026-07-23 (sesión #701):** Carlos indicó que el navegador integrado de
Claude Code Desktop (tools `mcp__Claude_Browser__*`: `preview_start`, `computer`,
`read_page`, `javascript_tool`, `read_network_requests`...) es la herramienta por
defecto para este tipo de verificación, más optimizada que Playwright MCP — que
queda reservado para otros usos. Ver `CLAUDE.md` §"Verificación de cambios en
navegador" (actualizado en la misma sesión).

En la verificación de #701 sobre la isla **expediente-arbol** (react-flow, expediente
AT-2004/id=4) con `mcp__Claude_Browser__computer{action:"screenshot"}` **no hubo cuelgue**:
capturas, `read_page`, `javascript_tool` y clicks funcionaron con normalidad sobre el nodo
ANALIZAR del trámite Requerimiento de Subsanación y su modal de requerimientos. La nota
histórica de abajo (Playwright, `preview_screenshot` colgado) viene de una sesión anterior
con **otra** herramienta de preview (no el navegador integrado actual) — mantenida por si
el problema reaparece con el navegador integrado en alguna isla concreta, pero ya no aplica
como regla general.

Login de dos pasos por fetch: ver [[project_login_dos_pasos]]. Expediente de prueba usado
en #701: AT-2004 (id=4), rol SUPERVISOR (id=2). Ruta de árbol: `/expedientes/<id>/arbol`
(nota: el código AT-NNNN mostrado en UI no coincide con el `id` interno de la tabla
`expedientes` — confirmar el id real antes de navegar, p. ej. AT-2004 es id=4).

---

## Nota histórica (sesión previa, herramienta de preview ya no vigente)

Verificar la isla **expediente-arbol** (react-flow) en navegador tenía varias trampas de
tooling con la herramienta de preview usada entonces:

- **`preview_screenshot` se CUELGA (timeout 30s) en islas que mantienen rAF/observers vivos**:
  confirmado en la vista de árbol (bucle `requestAnimationFrame` de react-flow) Y en la isla
  **estadisticas** (Recharts `ResponsiveContainer`, que monitoriza tamaño en bucle; ni siquiera
  `isAnimationActive={false}` lo evita). El renderer estaba vivo (`preview_snapshot` y
  `preview_eval` funcionaban siempre y bastaban para verificar estructura + valores).
- **El navegador de preview (`preview_start` "bddat", 5000) era HEADLESS**: el `Screenshot` de
  windows-mcp capturaba el escritorio real del usuario, no la preview.
- **CSSTransition se quedaba `pending` en `currentTime:0` en aquel preview headless**: medir
  `opacity` de un atenuado con transición devolvía el frame inicial, no el final.
- **`preview_click` (MCP) no siempre disparaba el `onClick` de React**; el click nativo sí.

Ver [[feedback_antibloqueos_bash]] para el resto de anti-bloqueos generales.
