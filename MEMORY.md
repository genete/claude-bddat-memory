# BDDAT — Memoria de proyecto

> **Nota:** Este fichero está fuera del control de versiones git.
> El historial de issues cerrados está en `git log` — no duplicar aquí.

## Feedback de trabajo
- [feedback_invariante_vs_regla_motor.md](feedback_invariante_vs_regla_motor.md) — Antes de implementar una salvaguarda: ¿invariante (hardcode), modelo legal (reglas_motor) o dato de catálogo? Decirlo en la propuesta; por defecto permitir con justificación, no bloquear
- [feedback_alembic_heads.md](feedback_alembic_heads.md) — Verificar `flask db current` antes de crear migración; si hay múltiples heads, fusionar primero
- [feedback_migracion_manual_no_autogenerado.md](feedback_migracion_manual_no_autogenerado.md) — Migraciones se escriben a mano desde el principio (`op.create_table` calcado al estilo del repo), nunca `flask db migrate` autogenerado + revisión
- [feedback_no_reintentar_latencia.md](feedback_no_reintentar_latencia.md) — Si un tool_result tarda, esperar; NO reemitir el mismo comando en bucle (encola ejecuciones reales)
- [feedback_skill_boe.md](feedback_skill_boe.md) — Usar siempre el skill /boe para leer legislación; no WebFetch por libre
- [feedback_partir_de_la_norma_no_de_la_implementacion.md](feedback_partir_de_la_norma_no_de_la_implementacion.md) — En cuestiones jurídicas leer la norma ANTES de proponer diseño; el código y los DISEÑO_*.md derivan de lecturas previas y arrastran premisas falsas (caso #788: «las fases tienen plazo»)
- [feedback_skill_legalize.md](feedback_skill_legalize.md) — /legalize solo por orden directa del usuario; /boe y /boja no lo llaman internamente
- [feedback_expansion_documentos.md](feedback_expansion_documentos.md) — Al escribir en docs de diseño, Claude expande con inferencias propias que no siempre están alineadas; requiere revisión
- [feedback_git_stash.md](feedback_git_stash.md) — No usar `git checkout --` para descartar cambios no relacionados del usuario
- [feedback_relectura_contexto.md](feedback_relectura_contexto.md) — No releer ficheros que ya están en contexto de la sesión actual
- [feedback_urgencia_desarrollo_vs_produccion.md](feedback_urgencia_desarrollo_vs_produccion.md) — BDDAT está en desarrollo, no en producción; no dar a los bugs urgencia de incidente salvo que Carlos lo pida
- [feedback_commits_atomicos_verificados.md](feedback_commits_atomicos_verificados.md) — En refactors grandes, commits atómicos verificados uno a uno, no un commit monolítico
- [feedback_antibloqueos_bash.md](feedback_antibloqueos_bash.md) — RESUELTO por hook `.claude/hooks/reglas_bash_guard.py`: deniega los anti-patrones. Si deniega, reescribir con el arreglo indicado; qué queda fuera de su cobertura
- [feedback_bash_vs_herramientas_dedicadas.md](feedback_bash_vs_herramientas_dedicadas.md) — No usar Bash para ls/find/grep/cat cuando Glob/Grep/Read cubren el caso, ni para consultas triviales; aprobar el permiso consolidaría el patrón en settings
- [feedback_milestones.md](feedback_milestones.md) — Issues relacionados van al mismo milestone que su dependencia, no por complejidad percibida
- [feedback_rm_temp.md](feedback_rm_temp.md) — Nunca borrar ficheros de temp/: dejarlos, el usuario los borra manualmente. rm y mv quedan bloqueados.
- [feedback_temp_nombre_unico.md](feedback_temp_nombre_unico.md) — Si el fichero temp destino ya existe, NO leerlo: crear uno con nombre distinto (sufijo issue/PR, -v2…) para no gastar tokens en contenido irrelevante
- [feedback_proactividad_tecnica.md](feedback_proactividad_tecnica.md) — Inferir el objetivo real, ofrecer alternativas técnicamente superiores antes de ejecutar lo pedido literalmente
- [feedback_vigencia_modificaciones_normativas.md](feedback_vigencia_modificaciones_normativas.md) — No confundir fecha original de norma con fecha de modificación concreta al evaluar vigencia frente a norma posterior
- [feedback_issues_en_memory.md](feedback_issues_en_memory.md) — No guardar en memoria estado de issues ni ramas activas; eso es de GitHub y git
- [feedback_commit_format.md](feedback_commit_format.md) — Formato de commits: `[CATEGORÍA] #N descripción`, NO conventional commits (feat:/fix:/docs:)
- [feedback_conformidad_implicita.md](feedback_conformidad_implicita.md) — No asumir conformidad implícita a sugerencias de diseño de Claude; esperar confirmación explícita antes de escribir en fuentes de verdad
- [feedback_sin_preferencia_no_es_indiferencia.md](feedback_sin_preferencia_no_es_indiferencia.md) — Varios "[No preference]" seguidos pueden señalar que las opciones parten de un encuadre que Carlos no comparte, no vía libre
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
- [feedback_worktree_venv_env.md](feedback_worktree_venv_env.md) — Worktree nuevo: venv propio sin pytest + falta .env + isla React sin bundle/node_modules (gitignored); smoke tests corren contra BD real, limpiar filas de alta con fixture autouse
- [feedback_no_worktrees_bddat.md](feedback_no_worktrees_bddat.md) — Carlos probó worktrees en #776 y decidió no usarlos en BDDAT; no ofrecerlos proactivamente de nuevo
- [feedback_worktree_settings_local_no_sync.md](feedback_worktree_settings_local_no_sync.md) — settings.local.json no sincroniza entre worktree y principal (Windows, confirmado); pero preguntas repetidas normalmente son por comandos exactos sin wildcard, no por venv en otra ruta
- [feedback_no_inventar_aislamiento_sin_ruta_herencia.md](feedback_no_inventar_aislamiento_sin_ruta_herencia.md) — No proponer aislamiento entre datos ficticios/reales sin verificar antes si existe ruta de herencia dev→prod
- [feedback_mapear_todo_aunque_implementacion_parcial.md](feedback_mapear_todo_aunque_implementacion_parcial.md) — Al planificar cobertura de cara a un hito, mapear todos los mecanismos aunque la implementación de algunos quede en placeholder
- [feedback_verificar_rama_antes_de_commit.md](feedback_verificar_rama_antes_de_commit.md) — Verificar rama git actual antes de commitear en sesiones largas; el snapshot inicial de gitStatus puede haber quedado obsoleto
- [feedback_docs_vivos_sin_issue.md](feedback_docs_vivos_sin_issue.md) — Ediciones a ADRs/docs de diseño vivos no necesitan issue ni rama feature/; commit directo a develop
- [feedback_contexto_actual_solo_tras_merge.md](feedback_contexto_actual_solo_tras_merge.md) — CONTEXTO_ACTUAL.md "Hecho" se actualiza solo tras mergear el PR, sin encolar histórico; y registra solo lo que NO está en ADRs, MDs ni issues
- [feedback_verificar_con_datos_reales_e_historial.md](feedback_verificar_con_datos_reales_e_historial.md) — Verificar con git log/show y datos de BD; pero la BD de desarrollo ilustra, no prueba (ficticia, desechable, con residuos previos a las salvaguardas). No revertir datos tras pruebas de UI
- [feedback_enganche_temprano_vs_tardio.md](feedback_enganche_temprano_vs_tardio.md) — Ante dos puntos de enganche candidatos, preferir el más temprano si el dato disparador ya se fija ahí (caso #657: clasificación en subida, no asociación a tarea)
- [feedback_estilos_odt_hijo_no_mutar.md](feedback_estilos_odt_hijo_no_mutar.md) — Variante de estilo ODT (p.ej. mayúsculas) va como estilo HIJO que solo añade la propiedad, nunca mutando el compartido; confirmar en qué plantilla vive el texto antes de tocar (#728: membrete vs título de resolución)
- [feedback_matar_proceso_flask_al_cerrar_navegador.md](feedback_matar_proceso_flask_al_cerrar_navegador.md) — Al terminar verificación en navegador, parar el run.py de background con **TaskStop sobre el task_id** (no taskkill/PowerShell: piden permiso), no solo browser_close
- [feedback_ruta_posix_arranque_run_py_allowlist.md](feedback_ruta_posix_arranque_run_py_allowlist.md) — Arrancar run.py con ruta POSIX (/d/BDDAT/...), nunca D:/BDDAT/..., para que coincida con la allowlist y no pida confirmación
- [feedback_atajos_interfaz_servicio.md](feedback_atajos_interfaz_servicio.md) — No añadir entradas «de conveniencia» a la interfaz de un servicio para niveles que el modelo dice que no tienen esa propiedad; si el consumidor llega por el nivel equivocado, la bajada es navegación suya
- [feedback_comandos_allowlist_verbatim.md](feedback_comandos_allowlist_verbatim.md) — Comandos rutinarios de la allowlist, VERBATIM: `npm --prefix /d/BDDAT/react-src run build` sin `| tail` ni `2>&1`, o rompe el match y pide permiso
- [feedback_traspaso_significa_parar.md](feedback_traspaso_significa_parar.md) — "Traspaso y finalizamos" es orden de parar; el mensaje automático de reanudación tras límite de uso no es de Carlos y no reactiva por sí solo el trabajo pendiente

## Sobre Carlos
- [user_prioridad_codigo_sobre_poblado.md](user_prioridad_codigo_sobre_poblado.md) — Prioriza issues que generan código; evita poblados puros de catálogo mientras pueda

## Stack
Python 3 + Flask + SQLAlchemy + PostgreSQL + Bootstrap 5.3 + Jinja2
Entorno virtual: `D:\BDDAT\venv\Scripts\activate`
Arranque servidor:
  - Usuario (manual, `!` en el prompt): `cd /d/BDDAT && source venv/Scripts/activate && python run.py`
  - Claude (Bash tool — sin source activate): `cd /d/BDDAT && venv/Scripts/python.exe run.py`
Credenciales Playwright: usuario `CLG`, contraseña `31416` — login de DOS pasos (multi-rol): ver [project_login_dos_pasos.md](project_login_dos_pasos.md)
- [feedback_puertos_zombis_windows_run_py.md](feedback_puertos_zombis_windows_run_py.md) — Windows permite varios `run.py` a la vez en el mismo puerto sin error; si un 500/404 persiste tras "reiniciar", comprobar procesos acumulados (PowerShell) antes de sospechar del código
- [project_reloj_dev_script.md](project_reloj_dev_script.md) — `scripts/reloj_dev.py` cambia la fecha simulada (#820) sin arrancar Flask; vocabulario de Carlos ("aumenta N días hábiles"...) → subcomando exacto

## Rama de trabajo
Siempre trabajar en `develop`. PRs siempre contra `develop`, no contra `main`.

## Ficheros temporales (commits, PRs)
NUNCA usar `/tmp/` — en Windows, `Write` y `bash` resuelven a rutas distintas.
Usar siempre `D:\BDDAT\docs_prueba\temp\` (allowlisted, gitignored). No borrar ni mover tras uso — `rm` y `mv` quedan bloqueados. El usuario los borra manualmente.

## Gestión de memoria y planes
- `C:\Users\carlo\.claude\projects\D--BDDAT\memory` — repositorio git gestionado autónomamente por el usuario
- `C:\Users\carlo\.claude\plans` — repositorio git gestionado autónomamente por el usuario
Claude no debe hacer commits ni gestionar git en estas rutas.

## MCPs configurados en Claude Code
- [project_mcps_configurados.md](project_mcps_configurados.md) — postgres (usuario claude_desktop, solo lectura), playwright, windows-mcp; perfil LGC005; comando windows-mcp corregido

## Herramientas de escritorio
- [project_libreoffice_headless.md](project_libreoffice_headless.md) — Invocar LibreOffice headless: `soffice.com` (el `.exe` se cuelga), perfil aparte, filtros de conversión; el render a PNG miente con los acentos

## Verificación en navegador
- [feedback_navegador_integrado_por_defecto.md](feedback_navegador_integrado_por_defecto.md) — SUPERSEDIDO 2026-08-04: Playwright MCP es ahora el default, sin preguntar (antes era el navegador integrado)
- [project_verif_arbol_react.md](project_verif_arbol_react.md) — Verificar árbol React con Playwright MCP (default desde 2026-08-04)
- [feedback_falso_positivo_navegador_integrado.md](feedback_falso_positivo_navegador_integrado.md) — Histórico (navegador integrado, ya no default): islas con panel/drawer dinámico podían dar coordenadas falsas
- [feedback_browser_pane_no_mostrado.md](feedback_browser_pane_no_mostrado.md) — Histórico (navegador integrado, ya no default): screenshot/coordinate fallaban si el panel no estaba visible en pantalla

## Arquitectura — árbol del expediente
- [project_arbol_crud_api.md](project_arbol_crud_api.md) — El CRUD del árbol vive en `api_expedientes.py` (/nodo/...), NO en `api_bc.py` (muerto desde #519); verificar el consumidor real antes de refactorizar permisos/comportamiento del árbol
- [feedback_umbral_factorizar_excepciones_bespoke.md](feedback_umbral_factorizar_excepciones_bespoke.md) — Factorizar una excepción de editor bespoke de tarea cuando comparte raíz con otra ya existente (no solo por recuento de apariciones); ver ocultarDespensa/#742 como ejemplo de caso único que aún no toca generalizar

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

## Arquitectura — plazos y suspensiones (art. 22 LPACAP)
- [project_aau_integrada_sin_causa_suspension.md](project_aau_integrada_sin_causa_suspension.md) — El bloque ambiental AAU_AAUS_INTEGRADA no suspende; desde #778 eso es una fila que falta en catalogo_plazos, no una lista que ampliar en el código

## Arquitectura — decisiones pendientes
- [project_plantilla_tarea_elaborar.md](project_plantilla_tarea_elaborar.md) — Asociación plantilla↔tarea-ELABORAR no implementada; evaluar tras probar CBs #392-#394 con plantilla dummy
- [project_fusion_n012_n013.md](project_fusion_n012_n013.md) — N012/N013 son en el fondo la misma necesidad (distinción por rol artificiosa); pendiente fusionar en catálogo, no como efecto colateral de otro issue
- [project_organizacion_documental_pendiente.md](project_organizacion_documental_pendiente.md) — Sesión de organización documental resuelta en ADR-032; #572 ortogonal pero diferido a propósito por Carlos, no por dependencia técnica
- [project_adr032_ingesta_documentos.md](project_adr032_ingesta_documentos.md) — ADR-032: dos vías de entrada al pool (in situ/multipart), rutas relativas, encaje por primera vinculación; regresión real detectada en #180
- [project_react_workbench_mutaciones.md](project_react_workbench_mutaciones.md) — Isla React workbench multiuso (árbol/listados/proyecto) + mutaciones en servicio reutilizable (camino B, extraer no reescribir); híbrido Jinja+React; archipiélago→ADR aparte
- [project_inspector_vs_modal_scripts.md](project_inspector_vs_modal_scripts.md) — Inspector (capa 2) NO re-ejecuta los <script> de fragmentos; modal grande (capa 3) SÍ → JS pesado en capa 3 o como delegación global (patrón ADR-023, migraciones de listados)

## Arquitectura — tarea ANALIZAR (confluencia #495/#581/#440/#442)
- [project_diseno_tarea_analizar_442.md](project_diseno_tarea_analizar_442.md) — Reparto inspector/modal (#495+#581 inline, #440 modal, #442 dueño del contenedor) y regla de cita normativa en defectos compilados: sin norma_id no hay placeholder, se omite sin más

## Ecosistema externo — integración bandeja / portafirmas
- [project_ecosistema_bandeja_token.md](project_ecosistema_bandeja_token.md) — Token BDDAT:<n> para recuperar nº comunicación de bandeja; Port@firmas opaco (scraping inviable); repos exploratorios bandeja-downloader/ptwanda-tramitador/notifica-poc

## Estado del proyecto
- [project_estado_mayo2026.md](project_estado_mayo2026.md) — Snapshot mayo 2026: core técnico construido, cuellos de botella en datos ESFTT y UI de segundo orden; huecos sin issue identificados
- [project_backend_solido_revamping.md](project_backend_solido_revamping.md) — El backend ESFTT absorbe las islas del revamping con casi pura lectura; estimar por lo que hay que construir, no por lo que la vista aparenta

## Arquitectura — notificaciones (NOTIFICAR)
- [project_adr034_notificaciones_tarea_id.md](project_adr034_notificaciones_tarea_id.md) — ADR-034: notificaciones ancla por tarea_id (no vitaminado de documento), dos caminos de escritura, justificante intermedio nunca es Documento, generalización BANDEJA/SIR

## Gestión de foco / clasificación de issues
- [project_clasificacion_issues_4bloques.md](project_clasificacion_issues_4bloques.md) — Esquema acordado: 4 bloques ortogonales multi-etiqueta (rol / tramitación directa / mantenimiento / residual) como clasificación primaria; zona de código pasa a eje técnico secundario
- [project_adr030_coverage_matrix_pattern.md](project_adr030_coverage_matrix_pattern.md) — Patrón de ADR-030 (matriz dimensión×valor con placeholders explícitos) aplicable más allá de tests: huecos por rol/motor sin issue filed son invisibles a cualquier clasificación de issues existentes
- [project_plan_estrategia_mapa_necesidades.md](project_plan_estrategia_mapa_necesidades.md) — PLAN_ESTRATEGIA §D/§G ya es el mapa de necesidades por rol y por milestone (confirma que los milestones siguen valiendo); §H revela deriva de la filosofía original de "issues mínimos" (2-3 activos), posible causa parcial del "efecto hidra"
