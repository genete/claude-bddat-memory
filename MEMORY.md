# BDDAT — Memoria de proyecto

> **Nota:** Este fichero está fuera del control de versiones git.
> El historial de issues cerrados está en `git log` — no duplicar aquí.

## Feedback de trabajo
- [feedback_skill_boe.md](feedback_skill_boe.md) — Usar siempre el skill /boe para leer legislación; no WebFetch por libre
- [feedback_skill_legalize.md](feedback_skill_legalize.md) — /legalize solo por orden directa del usuario; /boe y /boja no lo llaman internamente
- [feedback_expansion_documentos.md](feedback_expansion_documentos.md) — Al escribir en docs de diseño, Claude expande con inferencias propias que no siempre están alineadas; requiere revisión
- [feedback_git_stash.md](feedback_git_stash.md) — No usar `git checkout --` para descartar cambios no relacionados del usuario
- [feedback_relectura_contexto.md](feedback_relectura_contexto.md) — No releer ficheros que ya están en contexto de la sesión actual
- [feedback_antibloqueos_bash.md](feedback_antibloqueos_bash.md) — Claude tiende a olvidar verificar los anti-bloqueos Bash antes de escribir comandos, causando interrupciones evitables
- [feedback_milestones.md](feedback_milestones.md) — Issues relacionados van al mismo milestone que su dependencia, no por complejidad percibida
- [feedback_rm_temp.md](feedback_rm_temp.md) — Nunca borrar ficheros de temp/: dejarlos, el usuario los borra manualmente. rm y mv quedan bloqueados.
- [feedback_temp_nombre_unico.md](feedback_temp_nombre_unico.md) — Si el fichero temp destino ya existe, NO leerlo: crear uno con nombre distinto (sufijo issue/PR, -v2…) para no gastar tokens en contenido irrelevante
- [feedback_proactividad_tecnica.md](feedback_proactividad_tecnica.md) — Inferir el objetivo real, ofrecer alternativas técnicamente superiores antes de ejecutar lo pedido literalmente
- [feedback_vigencia_modificaciones_normativas.md](feedback_vigencia_modificaciones_normativas.md) — No confundir fecha original de norma con fecha de modificación concreta al evaluar vigencia frente a norma posterior
- [feedback_issues_en_memory.md](feedback_issues_en_memory.md) — No guardar en memoria estado de issues ni ramas activas; eso es de GitHub y git

## Stack
Python 3 + Flask + SQLAlchemy + PostgreSQL + Bootstrap 5.3 + Jinja2
Entorno virtual: `D:\BDDAT\venv\Scripts\activate`
Arranque servidor:
  - Usuario (manual, `!` en el prompt): `cd /d/BDDAT && source venv/Scripts/activate && python run.py`
  - Claude (Bash tool — sin source activate): `cd /d/BDDAT && venv/Scripts/python.exe run.py`
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
- [project_notebooklm_workflow.md](project_notebooklm_workflow.md) — Workflow NotebookLM+Claude para peinar normas AT; Drive en H:\Mi unidad\bddat-notebooklm\; hallazgos en docs/referencia/normas/hallazgos_nblm/; incluye paso de cruce regla a regla y nota de refactor pendiente

## Diseño experimental
- [project_mmd_diagrams.md](project_mmd_diagrams.md) — Diagrama MMD dinámico de ESFTT: API JSON agnóstica + estrategia híbrida dos diagramas
