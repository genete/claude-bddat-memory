# BDDAT — Memoria de proyecto

> **Nota:** Este fichero está fuera del control de versiones git.
> El historial de issues cerrados está en `git log` — no duplicar aquí.

## Stack
Python 3 + Flask + SQLAlchemy + PostgreSQL + Bootstrap 5.3 + Jinja2
Entorno virtual: `D:\BDDAT\venv\Scripts\activate`
Arranque servidor: `cd /d/BDDAT && source venv/Scripts/activate && python run.py`
Credenciales Playwright: usuario `CLG`, contraseña `31416`

## Rama de trabajo
Siempre trabajar en `develop`. PRs siempre contra `develop`, no contra `main`.

## MCPs configurados en Claude Code (~/.claude.json scope user)
- `postgres` — npx @modelcontextprotocol/server-postgres → bddat (usuario bddat_admin, propietario de las tablas)
  - `claude_desktop` es un usuario separado creado para Claude Desktop, sin relación con Claude Code
- `playwright` — npx @playwright/mcp@latest
- `windows-mcp` — uvx windows-mcp
Requieren reinicio de sesión para cargarse.

## Estructura modular activa
- app/modules/expedientes/ y app/modules/entidades/ — blueprints con template_folder
- app/routes/ — auth, dashboard, proyectos, usuarios, perfil, wizard_expediente, APIs

## Documentación de referencia
- `docs/GUIA_VISTAS_BOOTSTRAP.md` — tipos de vista y patrón V4 detalle/edición
- `docs/GUIA_COMPONENTES_INTERACTIVOS.md` — catálogo de componentes JS reutilizables
  (SelectorBusqueda, ScrollInfinito, etc.). Leer antes de implementar cualquier componente JS.
- `docs/fuentesIA/REGLAS_DESARROLLO.md` — workflow Git, commits, ramas, migraciones
- `docs/fuentesIA/GuiaGeneralNueva.md` — arquitectura general y lógica de negocio

## Tipos de vista (GUIA_VISTAS_BOOTSTRAP.md)
| Vista | Descripción |
|-------|-------------|
| V0 | Login (split-screen) |
| V1 | Dashboard (grid de cards) |
| V2 | Listado scroll infinito (C.1/C.2) |
| V3 | Tramitación acordeón |
| V4 | Detalle/Edición unificada |

## Patrón V4 — Detalle/Edición Unificada
Un solo template, parámetro `modo='ver'`/`'editar'` pasado desde la ruta.

- CERO salto de layout entre modos: mismos elementos HTML, solo cambia `readonly`
- Selects en modo ver → `<input type="text" readonly>` con texto formateado (mismo alto)
- Campos bloqueados → `data-bs-toggle="tooltip" title="..."`, NO texto subtítulo
- Form con `class="form-detail"` para activar focus ring verde corporativo
- Tooltips Bootstrap siempre inicializados (no solo en editar)
- Breadcrumb añade rama "Editar" en modo editar

### Clases CSS globales (v2-components.css)
- `.card-header-accent` — fondo verde apoyo corporativo + texto oscuro
- `.card-body-tinted` — fondo gris claro para destacar campos blancos
- `.form-control[readonly]` — fondo blanco (mismo aspecto que editable)
- `.form-detail .form-control:focus` — borde verde + halo corporativo

### ⚠️ text-muted en este proyecto = #bebebe (casi blanco)
Usar `fw-semibold` sin clase de color para etiquetas visibles. Nunca `text-muted` en labels.

## SelectorBusqueda — API correcta
- Método limpiar: `clear()` (NO `limpiar()`)
- Formato opciones: `{v: String(id), t: texto}` (NO `{id, text}`)
- Otros métodos: `getValue()`, `setValue(v,t)`, `setOpciones(opciones)`
- Carga dinámica en modales: fetch en evento `show.bs.modal`, pasar resultado a `setOpciones()`
- API autorizados devuelve `{data: [{id, text}]}` → convertir a `{v,t}` en JS antes de `setOpciones()`

## CDN Junta de Andalucía
- URL versionada: `https://cdn.juntadeandalucia.es/components/sass/1.2.5/css/`
- Archivos: `fonts.css`, `all.css` (FA 6.5.1), `custom-jda-bootstrap.css` (BS 5.3.3 verde nativo)
- JS: `https://cdn.juntadeandalucia.es/components/sass/1.2.5/js/bootstrap.bundle.min.js`
- Bootstrap Icons sigue en jsdelivr (no incluido en CDN Junta)
- `.btn-outline-primary` ya NO necesita override en v2-components.css (nativo en CDN)
- Los tres base templates usan el CDN: base_fullwidth.html, base_acordeon.html, base_login.html
- El CSS del CDN es CORS-blocked (no se puede inspeccionar via `cssRules` en DevTools)

### Override acordeones
El CDN sobreescribe Bootstrap y muestra ▼ para estado cerrado (confuso).
Override en `custom.css` al final: chevron-right (>) para cerrado, chevron-down (v) para abierto.
Aplica a todos los acordeones del proyecto.

## AutorizadoTitular — modelo
- Borrado lógico: `revocar()` / `restaurar()` (campo `activo`)
- Autoautorización implícita: el titular puede actuar por sí mismo sin entrada en BD
- `crear_autorizacion(titular_id, autorizado_id)` — lanza `ValueError` si ya existe activa
- Al crear desde ruta: reutilizar registro revocado si existe (`existente.restaurar()`)
  en vez de crear duplicado (respeta unique constraint de BD)
- Validators: solo verifican que la entidad exista y esté activa

## Módulo Entidades — estado actual
- `app/modules/entidades/routes.py`:
  - detalle(), editar() (GET/POST)
  - nueva_direccion(), editar_direccion(), toggle_direccion()
  - nueva_autorizacion(), revocar_autorizacion(), restaurar_autorizacion()
- `app/modules/entidades/templates/entidades/detalle.html` — template V4 unificado
  5 cards: Identificación, Contacto, Dirección, Direcciones de notificación, Autorizaciones
- `app/routes/api_entidades.py`:
  - GET /api/entidades — autocomplete + scroll infinito
  - GET /api/entidades/<id>/autorizados — autorizados vigentes (incluye al titular)
  - GET /api/entidades/<id>/candidatos-autorizacion — activas no autorizadas aún ({v,t})

## Motor de reglas — estado
Tablas en BD: `reglas_motor`, `condiciones_regla`, `tipos_solicitudes_compatibles`.
Modelos: `app/models/motor_reglas.py` (ReglaMotor, CondicionRegla, TipoSolicitudCompatible).
**Evaluador implementado** (PR #154, mergeado 2026-02-28): `app/services/motor_reglas.py`.
Documento de diseño: `docs/fuentesIA/MOTOR_REGLAS_arquitectura.md`.

**Vocabulario de eventos:** CREAR / INICIAR / FINALIZAR / BORRAR
CHECK constraint `reglas_motor.evento`: `('CREAR','INICIAR','FINALIZAR','BORRAR')`.

**API pública del evaluador:**
```python
from app.services.motor_reglas import evaluar
res = evaluar('INICIAR', 'FASE', entidad_id=45)
res = evaluar('CREAR',   'FASE', tipo_id=8, padre_id=23)
res = evaluar('BORRAR',  'TAREA', entidad_id=12)
# res.permitido, res.nivel ('BLOQUEAR'|'ADVERTIR'|''), res.mensaje, res.norma
```

**Checks hardcoded activos (invariantes de negocio):**
- BORRAR fase/tramite/tarea con `fecha_inicio` → BLOQUEAR
- BORRAR solicitud con fases iniciadas → BLOQUEAR
- FINALIZAR solicitud con fases abiertas → BLOQUEAR
- FINALIZAR fase con trámites abiertos → BLOQUEAR
- FINALIZAR trámite con tareas abiertas → BLOQUEAR
- FINALIZAR tarea sin doc requerido (según tipo) → BLOQUEAR

**Hooks en vista3.py:** INICIAR/FINALIZAR en editar_solicitud/fase/tramite/tarea.
**Rutas CREAR/BORRAR en vista3.py:** crear_fase, crear_tramite, crear_tarea, borrar_fase, borrar_tramite, borrar_tarea.
**Tablas vacías → PERMITIDO** (correcto para Fase 2; reglas las poblará el técnico).

**`figura_ambiental`** → existe como `proyecto.ia_id` FK → `tipos_ia`. Acceso motor: `proyecto.ia.siglas` (AAI, AAU, AAUS, CA, EXENTO).
**`es_modificacion`** → añadido a `proyectos` (Boolean, default=False, migración aplicada).

## Modelos ESFTT — notas clave
- `Solicitud.estado` es campo persistido (EN_TRAMITE, RESUELTA, DESISTIDA, ARCHIVADA)
- `Fase.resultado_fase_id` es FK a `tipos_resultados_fases` (NO boolean exito — GuiaGeneralNueva corregida)
- `Solicitud` tiene tipos via N:M `SolicitudTipo` (no tipo_solicitud_id directo)
- @property de estado implementadas (2026-02-28):
  - `Fase`: planificada / en_curso / finalizada / finalizada_favorable
  - `Tramite`: planificado / en_curso / finalizado
  - `Tarea`: planificada / en_curso / ejecutada / ejecutada_con_doc

## Subsistema documental — decisiones arquitectónicas
Ver `docs/fuentesIA/ARQUITECTURA_DOCUMENTOS.md` para el detalle completo.

- **Pool agnóstico confirmado:** `Documento` solo tiene FK expediente_id y tipo_doc_id. No tocar.
- **BDDAT SÍ genera URLs para ficheros subidos** — `pool_subir` guarda en `UPLOAD_FOLDER/expedientes/<id>/<uuid>/<nombre>` y almacena la ruta relativa en `Documento.url`. Para URLs externas (BOE, Notifica…) el usuario proporciona la URL via modal separado.
- **Particularización N:M** (NO herencia SQLAlchemy) — patrón único para extender documentos.
  Precedente: `DocumentoProyecto`. Próximo: `DocumentoOrganismo` (cuando llegue SEPARATAS).
- **tipos_documentos + tipo_doc_id:** ✅ implementado (#188). OTROS (id=1) = cajón de sastre.
- **Estado de consultas:** derivado en `app/services/seguimiento.py`, no persistido ni Event Sourcing.
- **Plazos:** servicio separado `app/services/plazos.py` (M3).
- **Generación de escritos (M2):** dos capas (ver `docs/fuentesIA/GUIA_CONTEXT_BUILDERS.md`).

### Modelo Documento — estado actual (tras #191, 2026-03-05)
- Campos **eliminados:** `origen`, `nombre_display`
- `fecha_administrativa` → **nullable**. NULL = pendiente de revisión O sin valor jurídico propio
  (borradores/informes internos). La API de tareas rechaza documentos con NULL si el tipo lo requiere.
- `prioridad`: 0=normal, >0=prioritario. Pseudo-bool, validación solo en frontend.
- Nombre a mostrar: siempre calculado desde la URL (último segmento sin extensión).
- Procedencia del emisor: en columnas de las tablas cualificadoras, no en Documento.

### Dos vías de entrada al pool (decisión 2026-03-05)
- **Vía 1 — Carga masiva** (#180): administrativo carga documentos al pool antes/durante tramitación.
  No genera tareas ESFTT. Es una operación de soporte, no un acto de tramitación.
- **Vía 2 — Tarea INCORPORAR**: solo documentos externos que llegan durante tramitación activa
  (organismos, alegantes, titular, BOE/BOP). El documento debe existir ya en el pool.
  **NO aplica a recepción inicial de solicitudes.**

### RECEPCION_SOLICITUD (cambio ESFTT v5.3)
Patrón G eliminado. RECEPCION_SOLICITUD usa ANALISIS (patrón A):
el técnico verifica el pool, cualifica documentos con tipo_doc_id y produce acta de recepción
como presupuesto para ADMISIBILIDAD y ANALISIS_TECNICO.

### ZIP — no válido como unidad de trabajo
Solo referencia histórica del paquete original de registro. Los documentos deben incorporarse
individualmente con tipo_doc_id para que el motor de reglas pueda trabajar con ellos.

### Orden de implementación
1. ✅ Pool del expediente — UI gestión masiva (#180, implementado: subida real de ficheros)
2. Selectores documento_usado / documento_producido en tareas (V3, #166)
3. Wizards y descubridores (flujos automatizados)
4. Requisitos documentales legales (#192, M5)

## Issues abiertos pendientes
- **#120** — Establecer base numero_at antes de producción (tarea pre-despliegue)
- **#152** — ✅ CERRADO — Motor de reglas evaluador implementado (PR #154, mergeado 2026-02-28)
- **#153** — [DRAFT] Consultas a organismos en separatas — tabla entidades_consultadas
