---
name: feedback_navegador_integrado_por_defecto
description: Usar el navegador integrado de Claude Code Desktop (mcp__Claude_Browser__*) por defecto para verificación en navegador; Playwright MCP queda para otros usos
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 20a08a7d-89b5-450c-b2f7-81761354099d
  modified: 2026-07-23T06:29:41.519Z
---

Para verificar cambios en navegador (dev server BDDAT, islas React, flujos e2e), usar por
defecto el **navegador integrado de Claude Code Desktop** (tools `mcp__Claude_Browser__*`:
`preview_start`, `computer`, `read_page`, `get_page_text`, `javascript_tool`,
`read_network_requests`...). No requiere pedir permiso cada vez.

**Por qué:** Carlos indicó (sesión #701, 2026-07-23) que esta herramienta está "mucho más
optimizada" para esta tarea que Playwright MCP, y que Playwright MCP "queda para otros usos".
Antes de esta indicación, la única guía en `CLAUDE.md` mencionaba Playwright MCP como
herramienta de testing en navegador con precaución de "preguntar siempre" — eso generaba duda
sobre qué herramienta usar. Se corrigió `CLAUDE.md` §"Verificación de cambios en navegador"
en la misma sesión para dejarlo explícito.

**Cómo aplicar:** ante cualquier tarea de verificación en navegador, ir directo a
`mcp__Claude_Browser__*` sin preguntar. Solo considerar Playwright MCP si el navegador
integrado falla en un caso concreto (y en ese caso, preguntar antes de usarlo, ver
[[project_verif_arbol_react]] para el historial de por qué esa precaución existía).
