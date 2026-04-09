# BDDAT — Memoria de proyecto

> **Nota:** Este fichero está fuera del control de versiones git.
> El historial de issues cerrados está en `git log` — no duplicar aquí.

## Feedback de trabajo
- [feedback_skill_boe.md](feedback_skill_boe.md) — Usar siempre el skill /boe para leer legislación; no WebFetch por libre
- [feedback_skill_legalize.md](feedback_skill_legalize.md) — /legalize solo por orden directa del usuario; /boe y /boja no lo llaman internamente
- [feedback_expansion_documentos.md](feedback_expansion_documentos.md) — Al escribir en docs de diseño, Claude expande con inferencias propias que no siempre están alineadas; requiere revisión
- [feedback_git_stash.md](feedback_git_stash.md) — No usar `git checkout --` para descartar cambios no relacionados del usuario
- [feedback_antibloqueos_bash.md](feedback_antibloqueos_bash.md) — Claude tiende a olvidar verificar los anti-bloqueos Bash antes de escribir comandos, causando interrupciones evitables
- [feedback_milestones.md](feedback_milestones.md) — Issues relacionados van al mismo milestone que su dependencia, no por complejidad percibida
- [feedback_rm_temp.md](feedback_rm_temp.md) — Nunca borrar ficheros de temp/: dejarlos, el usuario los borra manualmente. rm y mv quedan bloqueados.
- [feedback_proactividad_tecnica.md](feedback_proactividad_tecnica.md) — Inferir el objetivo real, ofrecer alternativas técnicamente superiores antes de ejecutar lo pedido literalmente
- [feedback_vigencia_modificaciones_normativas.md](feedback_vigencia_modificaciones_normativas.md) — No confundir fecha original de norma con fecha de modificación concreta al evaluar vigencia frente a norma posterior

## Stack
Python 3 + Flask + SQLAlchemy + PostgreSQL + Bootstrap 5.3 + Jinja2
Entorno virtual: `D:\BDDAT\venv\Scripts\activate`
Arranque servidor: `cd /d/BDDAT && source venv/Scripts/activate && python run.py`
Credenciales Playwright: usuario `CLG`, contraseña `31416`

## Rama de trabajo
Siempre trabajar en `develop`. PRs siempre contra `develop`, no contra `main`.

## Ficheros temporales (commits, PRs)
NUNCA usar `/tmp/` — en Windows, `Write` y `bash` resuelven a rutas distintas.
Usar siempre `D:\BDDAT\docs_prueba\temp\` (allowlisted, gitignored). No borrar ni mover tras uso — `rm` y `mv` quedan bloqueados. El usuario los borra manualmente.

## Gestión de memoria y planes
- `C:\Users\carlo\.claude\projects\D--BDDAT\memory` — repositorio git gestionado autónomamente por el usuario
- `C:\Users\carlo\.claude\plans` — repositorio git gestionado autónomamente por el usuario
Claude no debe hacer commits ni gestionar git en estas rutas.

## MCPs configurados en Claude Code (~/.claude.json scope user)
- `postgres` — npx @modelcontextprotocol/server-postgres → bddat (usuario bddat_admin)
- `playwright` — npx @playwright/mcp@latest
- `windows-mcp` — uvx windows-mcp
Requieren reinicio de sesión para cargarse.

## NotebookLM — extracción normativa
- [project_notebooklm_workflow.md](project_notebooklm_workflow.md) — Workflow NotebookLM+Claude para peinar normas AT; Drive en H:\Mi unidad\bddat-notebooklm\; hallazgos en docs/normas/hallazgos_nblm/

## Integración legalize-es (completado 2026-04-05)
- [project_legalize_es_pendiente.md](project_legalize_es_pendiente.md) — legalize-es en D:\legalize-es; skills /legalize (local), /boe (estatal), /boja (andaluz)

## Diseño experimental (sin issue aún)
- [project_mmd_diagrams.md](project_mmd_diagrams.md) — Diagrama MMD dinámico de ESFTT: API JSON agnóstica + estrategia híbrida dos diagramas

## Issues en curso
- [project_263_contexto.md](project_263_contexto.md) — fix/263 bugs seguimiento: orden y decisiones de diseño

## Decisiones arquitecturales pendientes de implementar
- [project_motor_rediseno_2026_03_31.md](project_motor_rediseno_2026_03_31.md) — Motor agnóstico + ContextAssembler + controlador de fechas: decisiones, deudas y orden de trabajo (sesión 2026-03-31)

## Documentación de referencia
- `docs/GUIA_VISTAS_BOOTSTRAP.md` — tipos de vista y patrón V4 detalle/edición
- `docs/GUIA_COMPONENTES_INTERACTIVOS.md` — componentes JS (SelectorBusqueda, ScrollInfinito, FiltrosListado). Leer antes de implementar JS.
- `docs/REGLAS_DESARROLLO.md` — workflow Git, commits, ramas, migraciones
- `docs/GUIA_GENERAL.md` — arquitectura general y lógica de negocio
- `docs/REGLAS_BASH.md` — patrones prohibidos en Bash y workarounds

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
