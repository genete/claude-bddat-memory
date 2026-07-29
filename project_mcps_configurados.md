---
name: project_mcps_configurados
description: "MCPs configurados en Claude Code scope user (postgres/playwright/windows-mcp) — perfil LGC005, comandos verificados"
metadata: 
  node_type: memory
  type: project
  originSessionId: 51c035ba-6fa2-4d0a-8c32-09512a4366bf
  modified: 2026-07-23T12:56:27.599Z
---

Configuración actual en `C:\Users\LGC005\.claude.json` (scope `user`, no `.mcp.json` de proyecto). Reconstruida el 2026-07-23 tras detectar que el perfil de Windows había cambiado de `C:\Users\carlo\` a `C:\Users\LGC005\` y la config de MCPs no había migrado — `claude mcp list` devolvía vacío pese a que la documentación previa la daba por viva.

**Why:** el cambio de perfil de Windows deja `~/.claude.json` en blanco; cualquier MCP con scope `user` documentado en memoria antigua hay que darlo por no instalado hasta comprobarlo con `claude mcp list`, no asumir que sigue vivo.

- **postgres** — `npx -y @modelcontextprotocol/server-postgres postgresql://claude_desktop:31416@localhost:5432/bddat`
  Usuario **`claude_desktop`** (solo lectura), NO `bddat_admin`. Verificado por consulta directa a `pg_roles` / `information_schema.role_table_grants`: `rolcanlogin=t`, `rolsuper=f`, `rolcreaterole=f`, `rolcreatedb=f`; `SELECT` sobre las 67 tablas base de `public` (cobertura completa, sin vistas adicionales), `USAGE` sin `CREATE` sobre el esquema. Sin `ALTER DEFAULT PRIVILEGES` configurado: **tablas nuevas de futuras migraciones Alembic no heredan SELECT automáticamente**, hay que concederlo a mano cada vez (o configurar el default privilege si se quiere automático).
- **playwright** — `npx @playwright/mcp@latest` (sin cambios respecto a la config previa)
- **windows-mcp** — `uvx windows-mcp serve --transport stdio`. El comando antiguo (`uvx windows-mcp` a secas) está **mal** — el paquete requiere el subcomando `serve`; sin él solo imprime el `--help` y sale, `claude mcp list` lo marca como "Failed to connect".

**How to apply:** si un MCP con scope user aparece no disponible en una sesión, comprobar primero con `claude mcp list` antes de asumir que hay que tocar código o BD — puede ser simplemente que el perfil de Windows cambió. Para postgres, usar siempre `claude_desktop` salvo que la tarea requiera escritura explícita (en cuyo caso preguntar antes de usar `bddat_admin`). Tras cualquier `claude mcp add/remove`, la sesión actual no ve las tools nuevas hasta reiniciar Claude Code.
