---
name: feedback_navegador_integrado_por_defecto
description: "SUPERSEDIDO 2026-08-04: Playwright MCP es ahora la herramienta por defecto de verificación en navegador, no el navegador integrado de Claude Code Desktop"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 20a08a7d-89b5-450c-b2f7-81761354099d
  modified: 2026-08-04T11:57:33.105Z
---

**Revertido 2026-08-04 (commit `1bda33a`, sesión #630/PR #756).** Carlos cambió `CLAUDE.md`
§"Verificación de cambios en navegador": ahora **Playwright MCP** es la herramienta por
defecto, sin preguntar — justo lo contrario de lo que decía esta memoria desde la sesión
#701 (2026-07-23).

**Por qué el cambio:** validado en vivo contra el árbol AT-2004 (react-flow). Playwright
resuelve el "Browser pane is not displayed" del navegador integrado (gap de producto
confirmado, sin ajuste disponible — ver `anthropics/claude-code#51587`,
[[feedback_browser_pane_no_mostrado]]) y expone nodos custom sin rol ARIA vía accessibility
snapshot + selectores estables (`data-testid`), sin depender de coordenadas de pantalla.
`browser_resize` verificado en ambos sentidos — viewport real, independiente de la ventana
física. Con tanto trabajo en islas React, probar primero el navegador integrado para acabar
recurriendo a Playwright duplicaba login/navegación sin necesidad.

**Cómo aplicar ahora:** ante cualquier tarea de verificación en navegador, ir directo a
`mcp__playwright__*` (tools deferred — cargar con ToolSearch) sin preguntar. Login de dos
pasos: [[project_login_dos_pasos]]. Capturas: sin nombre → auto-genera en `.playwright-mcp/`;
con nombre propio, **siempre** prefijar `.playwright-mcp/nombre.png`. Cerrar el navegador con
`browser_close` al terminar.

El navegador integrado (`mcp__Claude_Browser__*`) queda sin mención en `CLAUDE.md` tras este
cambio — si se necesita para algo puntual, tratarlo como excepción a confirmar con Carlos, no
como alternativa por defecto (inversión del criterio anterior). Ver [[project_verif_arbol_react]]
para el detalle de qué fallaba exactamente con el navegador integrado.
