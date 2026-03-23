# BDDAT — Memoria de proyecto

> **Nota:** Este fichero está fuera del control de versiones git.
> El historial de issues cerrados está en `git log` — no duplicar aquí.

## Feedback de trabajo
- [feedback_git_stash.md](feedback_git_stash.md) — No usar `git checkout --` para descartar cambios no relacionados del usuario
- [feedback_antibloqueos_bash.md](feedback_antibloqueos_bash.md) — Claude tiende a olvidar verificar los anti-bloqueos Bash antes de escribir comandos, causando interrupciones evitables

## Stack
Python 3 + Flask + SQLAlchemy + PostgreSQL + Bootstrap 5.3 + Jinja2
Entorno virtual: `D:\BDDAT\venv\Scripts\activate`
Arranque servidor: `cd /d/BDDAT && source venv/Scripts/activate && python run.py`
Credenciales Playwright: usuario `CLG`, contraseña `31416`

## Rama de trabajo
Siempre trabajar en `develop`. PRs siempre contra `develop`, no contra `main`.

## Ficheros temporales (commits, PRs)
NUNCA usar `/tmp/` — en Windows, `Write` y `bash` resuelven a rutas distintas.
Usar siempre `D:\BDDAT\docs_prueba\temp\` (allowlisted, gitignored). Borrar tras uso.

## MCPs configurados en Claude Code (~/.claude.json scope user)
- `postgres` — npx @modelcontextprotocol/server-postgres → bddat (usuario bddat_admin)
- `playwright` — npx @playwright/mcp@latest
- `windows-mcp` — uvx windows-mcp
Requieren reinicio de sesión para cargarse.

## Documentación de referencia
- `docs/GUIA_VISTAS_BOOTSTRAP.md` — tipos de vista y patrón V4 detalle/edición
- `docs/GUIA_COMPONENTES_INTERACTIVOS.md` — componentes JS (SelectorBusqueda, ScrollInfinito, FiltrosListado). Leer antes de implementar JS.
- `docs/fuentesIA/REGLAS_DESARROLLO.md` — workflow Git, commits, ramas, migraciones
- `docs/fuentesIA/GuiaGeneralNueva.md` — arquitectura general y lógica de negocio
- `docs/fuentesIA/ANTI_BLOQUEOS_BASH.md` — patrones prohibidos en Bash y workarounds

## CDN Junta de Andalucía
- CSS: `https://cdn.juntadeandalucia.es/components/sass/1.2.5/css/` (`fonts.css`, `all.css`, `custom-jda-bootstrap.css`)
- JS: `https://cdn.juntadeandalucia.es/components/sass/1.2.5/js/bootstrap.bundle.min.js`
- Bootstrap Icons en jsdelivr (no incluido en CDN)
- CDN es CORS-blocked — no inspeccionable via `cssRules` en DevTools
- Override acordeones en `custom.css`: chevron-right cerrado, chevron-down abierto

## Trampas conocidas
- **`text-muted` = #bebebe** (casi blanco). Usar `fw-semibold` sin color en labels visibles. Nunca `text-muted` en labels.
- **AutorizadoTitular:** al crear desde ruta, reutilizar registro revocado si existe (`existente.restaurar()`) en vez de crear duplicado — respeta unique constraint.
- **`SelectorBusqueda`:** método limpiar es `clear()` (NO `limpiar()`). Formato opciones: `{v: String(id), t: texto}`.
