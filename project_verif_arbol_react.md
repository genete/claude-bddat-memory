---
name: project_verif_arbol_react
description: "Cómo verificar la isla React del árbol de expediente (react-flow) en navegador con Playwright MCP; troubleshooting heredado del navegador integrado (ya no default, CLAUDE.md lo fija)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 0b057c39-2c1e-4162-9252-46aa9b4caade
  modified: 2026-08-29T07:45:02.312Z
---

`CLAUDE.md` ya fija Playwright MCP como herramienta por defecto de verificación en navegador
(sin preguntar) — no repetir esa regla aquí. Esta memoria guarda el detalle operativo de la
isla **expediente-arbol** (react-flow) y el troubleshooting que llevó a esa decisión.

**Datos de la isla:** login de dos pasos por fetch, ver [[project_login_dos_pasos]]. Expediente
de prueba usado en las sesiones de referencia: AT-2004 (id=4), rol SUPERVISOR (id=2). Ruta:
`/expedientes/<id>/arbol`. **El código AT-NNNN mostrado en UI no coincide con el `id` interno**
de la tabla `expedientes` — confirmar el id real antes de navegar (AT-2004 es id=4).

Capturas con Playwright: sin nombre → auto-genera en `.playwright-mcp/`; con nombre propio,
**siempre** prefijar `.playwright-mcp/nombre.png`. Cerrar con `browser_close` al terminar.

## Por qué se abandonó el navegador integrado (`mcp__Claude_Browser__*`) como default

Cambio decidido 2026-08-04 (commit `1bda33a`, sesión #630/PR #756) tras dos fallos reales:

- **"Browser pane is not displayed"**: `screenshot`/clicks por `coordinate` fallan cuando el
  panel del navegador integrado no está visualmente mostrado en la interfaz de Claude Code
  Desktop — es un estado de la sesión/cliente (gap de producto confirmado, sin ajuste
  disponible, ver `anthropics/claude-code#51587`), no un límite de la isla React probada. No
  hay tool para forzar que el panel se muestre — depende de que el usuario lo tenga abierto.
  Si aparece, no reintentar en bucle: usar `javascript_tool` (dispatchEvent sobre
  `querySelector`) + `get_page_text`/DOM para confirmar wiring sin necesitar captura visual.
- **Falsos positivos de posición en paneles dinámicos**: un botón del Inspector de
  `tablas_maestras` se midió con `getBoundingClientRect().x ≈ 2065px` (fuera de un viewport de
  1280px) en el navegador integrado, pero Carlos no lo reproducía en su navegador real. Ni
  siquiera medir con JS directo (no solo con la tool de clic) descarta el artefacto — paneles
  con `transform`/posicionamiento dinámico (drawers deslizantes) pueden renderizar mal ahí sin
  que sea un bug real de la app. Contrastar con Carlos antes de dar un hallazgo así por
  "confirmado" con una sola reproducción propia.

Playwright resuelve ambos (accessibility snapshot + selectores estables como `data-testid` en
vez de coordenadas de pantalla) y expone nodos custom sin rol ARIA (los `.arbol-tarea` de
react-flow son `<div>` con handler React, no elementos accesibles — `read_page` no los
etiqueta en el navegador integrado).

**Si alguna vez hace falta el navegador integrado para algo puntual**, tratarlo como excepción
a confirmar con Carlos, no como alternativa por defecto.
