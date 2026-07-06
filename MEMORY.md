# BDDAT — Memoria de proyecto

> **Nota:** Este fichero está fuera del control de versiones git.
> El historial de issues cerrados está en `git log` — no duplicar aquí.

## Feedback de trabajo
- [feedback_alembic_heads.md](feedback_alembic_heads.md) — Verificar `flask db current` antes de crear migración; si hay múltiples heads, fusionar primero
- [feedback_no_reintentar_latencia.md](feedback_no_reintentar_latencia.md) — Si un tool_result tarda, esperar; NO reemitir el mismo comando en bucle (encola ejecuciones reales)
- [feedback_skill_boe.md](feedback_skill_boe.md) — Usar siempre el skill /boe para leer legislación; no WebFetch por libre
- [feedback_skill_legalize.md](feedback_skill_legalize.md) — /legalize solo por orden directa del usuario; /boe y /boja no lo llaman internamente
- [feedback_expansion_documentos.md](feedback_expansion_documentos.md) — Al escribir en docs de diseño, Claude expande con inferencias propias que no siempre están alineadas; requiere revisión
- [feedback_git_stash.md](feedback_git_stash.md) — No usar `git checkout --` para descartar cambios no relacionados del usuario
- [feedback_relectura_contexto.md](feedback_relectura_contexto.md) — No releer ficheros que ya están en contexto de la sesión actual
- [feedback_commits_atomicos_verificados.md](feedback_commits_atomicos_verificados.md) — En refactors grandes, commits atómicos verificados uno a uno, no un commit monolítico
- [feedback_antibloqueos_bash.md](feedback_antibloqueos_bash.md) — Claude tiende a olvidar verificar los anti-bloqueos Bash antes de escribir comandos, causando interrupciones evitables
- [feedback_milestones.md](feedback_milestones.md) — Issues relacionados van al mismo milestone que su dependencia, no por complejidad percibida
- [feedback_rm_temp.md](feedback_rm_temp.md) — Nunca borrar ficheros de temp/: dejarlos, el usuario los borra manualmente. rm y mv quedan bloqueados.
- [feedback_temp_nombre_unico.md](feedback_temp_nombre_unico.md) — Si el fichero temp destino ya existe, NO leerlo: crear uno con nombre distinto (sufijo issue/PR, -v2…) para no gastar tokens en contenido irrelevante
- [feedback_proactividad_tecnica.md](feedback_proactividad_tecnica.md) — Inferir el objetivo real, ofrecer alternativas técnicamente superiores antes de ejecutar lo pedido literalmente
- [feedback_vigencia_modificaciones_normativas.md](feedback_vigencia_modificaciones_normativas.md) — No confundir fecha original de norma con fecha de modificación concreta al evaluar vigencia frente a norma posterior
- [feedback_issues_en_memory.md](feedback_issues_en_memory.md) — No guardar en memoria estado de issues ni ramas activas; eso es de GitHub y git
- [feedback_commit_format.md](feedback_commit_format.md) — Formato de commits: `[CATEGORÍA] #N descripción`, NO conventional commits (feat:/fix:/docs:)
- [feedback_conformidad_implicita.md](feedback_conformidad_implicita.md) — No asumir conformidad implícita a sugerencias de diseño de Claude; esperar confirmación explícita antes de escribir en fuentes de verdad
- [feedback_analisis_impacto.md](feedback_analisis_impacto.md) — Ante refactorizaciones: presentar tabla de consumidores en TODO el sistema antes de escribir código
- [feedback_una_conversacion_por_issue.md](feedback_una_conversacion_por_issue.md) — Una sesión = un issue; no continuar con el siguiente en la misma conversación
- [feedback_fuentes_verdad.md](feedback_fuentes_verdad.md) — No tratar derivados como consumidores independientes; si el JSON ya es correcto, los derivados solo necesitan /sync-derivados
- [feedback_git_rm_modelos.md](feedback_git_rm_modelos.md) — Al eliminar ficheros del proyecto, usar git rm en el mismo issue; no dejarlos como código muerto sin importar
- [feedback_plantilla_dummy_cb.md](feedback_plantilla_dummy_cb.md) — No crear plantilla .docx dummy para CBs; no tiene uso real ni en dev ni en prod
- [feedback_pr_refs_vs_closes.md](feedback_pr_refs_vs_closes.md) — En issues multi-fase usar `Refs #N`, nunca `Closes #N`, hasta el PR de cierre definitivo
- [feedback_rama_antes_de_empezar.md](feedback_rama_antes_de_empezar.md) — Crear rama feature/ ANTES de la primera edición; nunca commitear en develop y rectificar después
- [feedback_checklist_body_issue.md](feedback_checklist_body_issue.md) — Tareas/checklists pendientes van al cuerpo del issue, no a comentarios (se le pasan al revisar; y los checkbox del body cuentan progreso)
- [feedback_consolidar_antes_de_nuevo.md](feedback_consolidar_antes_de_nuevo.md) — Orden de roadmap: consolidar infra + migrar lo existente antes de construir vistas nuevas aisladas; distinguir lo preceptivo de lo importante
- [feedback_verificar_atribucion_codigo.md](feedback_verificar_atribucion_codigo.md) — Al mapear código→catálogo por grep, confirmar la clase real tras un `.codigo` (puede ser un dataclass interno, no un modelo) antes de darlo por bueno
- [feedback_emplazamiento_navegacion.md](feedback_emplazamiento_navegacion.md) — Al dar de alta una pantalla admin nueva, el patrón de UI a copiar y su emplazamiento de navegación son decisiones distintas; aplicar el test de ADR-029, no copiar el módulo hermano más parecido
- [feedback_worktree_venv_env.md](feedback_worktree_venv_env.md) — Worktree nuevo: venv propio sin pytest + falta .env (gitignored); smoke tests corren contra BD real, limpiar filas de alta con fixture autouse

## Stack
Python 3 + Flask + SQLAlchemy + PostgreSQL + Bootstrap 5.3 + Jinja2
Entorno virtual: `D:\BDDAT\venv\Scripts\activate`
Arranque servidor:
  - Usuario (manual, `!` en el prompt): `cd /d/BDDAT && source venv/Scripts/activate && python run.py`
  - Claude (Bash tool — sin source activate): `cd /d/BDDAT && venv/Scripts/python.exe run.py`
Credenciales Playwright: usuario `CLG`, contraseña `31416` — login de DOS pasos (multi-rol): ver [project_login_dos_pasos.md](project_login_dos_pasos.md)

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

## Verificación en navegador
- [project_verif_arbol_react.md](project_verif_arbol_react.md) — Vista de árbol React: preview_screenshot se cuelga (rAF react-flow), usar Playwright MCP visible; preview headless; transición pending; click nativo en vez de preview_click

## Arquitectura — árbol del expediente
- [project_arbol_crud_api.md](project_arbol_crud_api.md) — El CRUD del árbol vive en `api_expedientes.py` (/nodo/...), NO en `api_bc.py` (muerto desde #519); verificar el consumidor real antes de refactorizar permisos/comportamiento del árbol

## Catálogo de documentos
- [project_catalogo_documentos_alcance.md](project_catalogo_documentos_alcance.md) — Alcance del catálogo: cubre flujo ESFTT, excluye documentación presentada; patrón DR_NO_DUP para tipos del motor

## NotebookLM — extracción normativa
- [project_notebooklm_workflow.md](project_notebooklm_workflow.md) — Workflow NotebookLM+Claude para peinar normas AT; Drive en H:\Mi unidad\bddat-notebooklm\; hallazgos en docs/referencia/normas/hallazgos_nblm/; incluye paso de cruce regla a regla y nota de refactor pendiente

## Ecosistema externo (managers)
- [project_ptwanda_estudio_dom.md](project_ptwanda_estudio_dom.md) — Estudio DOM de PTWANDA para ptwanda-manager: acceso DNI/pass (no cert), bloqueo permanente (liberar navegando a inicio), descarga por refdoc; doc vivo en docs/referencia/ESTUDIO_DOM_PTWANDA.md

## Diseño experimental
- [project_mmd_diagrams.md](project_mmd_diagrams.md) — Diagrama MMD dinámico de ESFTT: API JSON agnóstica + estrategia híbrida dos diagramas

## Arquitectura — motor y bloqueos entre fases
- [project_bloqueos_naturales_vs_motor.md](project_bloqueos_naturales_vs_motor.md) — Bloqueo de motor (tasa, universal) vs imposibilidad natural por falta de documento (separata→Consultas, EIA→AAU integrada); esto último no lleva issue propio
- [project_capas_catalogo_motor.md](project_capas_catalogo_motor.md) — 3 capas independientes en tipos_fases/tramites/tareas: vocabulario (libre), reglas_motor (dato, futuro #170/#171), casos especiales (código); generalizar Variable Registry evaluado y descartado

## Arquitectura — decisiones pendientes
- [project_plantilla_tarea_elaborar.md](project_plantilla_tarea_elaborar.md) — Asociación plantilla↔tarea-ELABORAR no implementada; evaluar tras probar CBs #392-#394 con plantilla dummy
- [project_react_workbench_mutaciones.md](project_react_workbench_mutaciones.md) — Isla React workbench multiuso (árbol/listados/proyecto) + mutaciones en servicio reutilizable (camino B, extraer no reescribir); híbrido Jinja+React; archipiélago→ADR aparte
- [project_inspector_vs_modal_scripts.md](project_inspector_vs_modal_scripts.md) — Inspector (capa 2) NO re-ejecuta los <script> de fragmentos; modal grande (capa 3) SÍ → JS pesado en capa 3 o como delegación global (patrón ADR-023, migraciones de listados)

## Ecosistema externo — integración bandeja / portafirmas
- [project_ecosistema_bandeja_token.md](project_ecosistema_bandeja_token.md) — Token BDDAT:<n> para recuperar nº comunicación de bandeja; Port@firmas opaco (scraping inviable); repos exploratorios bandeja-downloader/ptwanda-tramitador/notifica-poc

## Estado del proyecto
- [project_estado_mayo2026.md](project_estado_mayo2026.md) — Snapshot mayo 2026: core técnico construido, cuellos de botella en datos ESFTT y UI de segundo orden; huecos sin issue identificados
- [project_backend_solido_revamping.md](project_backend_solido_revamping.md) — El backend ESFTT absorbe las islas del revamping con casi pura lectura; estimar por lo que hay que construir, no por lo que la vista aparenta
