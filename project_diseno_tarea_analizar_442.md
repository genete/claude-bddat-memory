---
name: project_diseno_tarea_analizar_442
description: Diseño acordado (sesión 2026-07-06) de cómo confluyen
metadata: 
  node_type: memory
  type: project
  originSessionId: abfba370-8a9d-4493-8ae0-86265d7fca79
---

Sesión 2026-07-06, al analizar el alcance de #581, se acordó cómo encajan en
ADR-023 (seleccionar→inspector→modal) los cuatro issues que confluyen en la
pantalla de la tarea ANALIZAR:

## Reparto de capas (criterio ADR-023 §6: escalar/campo → inspector,
## colección con CRUD propio → modal grande)

- **#495 (check documental)** y **#581 (check ítems técnicos)**: lista fija
  del catálogo, por fila solo se rellena `texto`+`cubierto` — no hay alta/baja
  de filas por el técnico. Van **inline en el inspector** como secciones
  expandibles, no como modal.
- **#440 (selector de requerimientos, shuttle)**: sí es un CRUD real (buscar
  en catálogo, texto libre nuevo, reordenar, persistir en catálogo) — va a
  **modal grande**, el único de los tres mecanismos que lo necesita.
- **#442 (formulario diagnóstico)**: pasa de "formulario suelto" a **dueño
  del fragmento contenedor** del inspector — layout, consolidación de los
  defectos que aportan los tres mecanismos, campo escalar `resultado`
  (favorable/desfavorable) y el botón de producir el documento de
  diagnóstico. #495/#581 aportan cada uno su sección inline; #440 aporta el
  modal.

Al cerrar el modal de #440, el inspector se refresca (comportamiento estándar
de ADR-023 §6) y el listado consolidado de defectos se recalcula solo.

## Regla de composición de texto de defectos (cita normativa)

Tres orígenes con estructura de datos distinta:

- **Directos** (#440: catálogo `catalogo_requerimientos` o texto libre) — el
  modelo **no tiene** `norma_id`/`articulo`, solo `texto`. Nunca se compone
  cita automática; si el técnico quiere citar la norma la escribe a mano.
- **Compilado documental** (#495) — `RequisitoDocumental.norma_id` (nullable)
  + `articulo`, separados de `descripcion_legal`.
- **Compilado técnico** (#581) — `ItemTecnico.norma_id` (nullable) +
  `articulo`, separados de `descripcion`.

Regla acordada para los dos compilados: **se añade la cita solo si
`norma_id` está relleno; si no está, se omite el texto del defecto sin más
— nunca un placeholder tipo "norma no identificada" o "sin norma".**

**Por qué:** que `norma_id` esté vacío no significa que el requisito sea
arbitrio administrativo — puede que la norma exista realmente pero no se
haya cargado aún en la tabla (población de #595/#408 incompleta). Un
placeholder visible daría la impresión falsa de discrecionalidad
administrativa donde en realidad hay una norma real pendiente de catalogar.

Formato propuesto (pendiente de confirmar en construcción):
`descripcion + f" (art. {articulo or '—'}, {norma.titulo})"` solo si
`norma_id` existe; si no, `descripcion` a secas.

**Riesgo a documentar en la UI de Supervisor** (#594/#408): si el Supervisor
ya redacta la cita a mano dentro de `descripcion`/`descripcion_legal`, saldría
duplicada al compilar — advertir en la pantalla de edición del catálogo.

## Corrección 2026-07-09: documento único de salida (no dos mecanismos)

Al retomar #440/#495/#581, Carlos detectó una costura sin cerrar entre dos
piezas construidas en momentos distintos sin remirarse:

- `DISEÑO_ANALISIS_SOLICITUD.md` §6 (fechado 21/03/2026) diseñó
  `ContextoSubsanacion` (#406, 2026-05-20) para leer `requerimientos_tarea`
  **directamente** y construir el escrito de REQUERIMIENTO_SUBSANACIÓN — en
  esa fecha el `Diagnostico` de #392 (2026-05-15) todavía no se pensaba como
  punto de consolidación.
- `consolidar_defectos()` (#442, 2026-07-07) sí unifica documental+técnico+
  general en `Diagnostico.defectos`, con `Diagnostico.as_contexto_cb()`
  (ya existía desde #392) pensado exactamente para alimentar context
  builders — pero `ContextoSubsanacion` nunca se migró a usarlo.

**Bug real, no cosmético:** un defecto documental o técnico que no se
reescriba a mano en el shuttle de #440 nunca llega al escrito notificado al
interesado, aunque sí aparece en el diagnóstico interno.

**Modelo correcto (confirmado por Carlos):** documento único de salida.
Las tablas de borrador (`documentos_requisito` para documental, una nueva
para técnico, `requerimientos_tarea` para general) son solo estado de
trabajo interno de la tarea ANALIZAR. `consolidar_defectos()` las agrega en
memoria; `Diagnostico.defectos` (vía `crear_diagnostico`) es la única foto
congelada de salida. Todo lo posterior —incluido `ContextoSubsanacion`— debe
leer de ahí (`diagnostico.as_contexto_cb()`), nunca releer las tablas de
borrador.

**Decisión de alcance:** el fix de `ContextoSubsanacion` (dejar de leer
`requerimientos_tarea`, pasar a leer el `Diagnostico` del ANALIZAR anterior)
se mete **dentro de #440** — es quien primero tropieza con el hueco al
construir el shuttle, es su hueco natural, no un issue aparte.

## Persistencia en disco de los tres ejes (2026-07-09)

Ninguno de los tres mecanismos vive "solo en memoria del navegador" hasta
producir el diagnóstico — cada uno persiste en su propia tabla en el momento
en que el técnico actúa, no en el botón "Producir":

- **Documental** — `documentos_requisito` (#192). No hay columna check: la
  fila (única por `requisito_id`+`solicitud_id`) **es** el check. Sin
  comentario — si falta, el texto sale de `descripcion_legal` del catálogo.
- **Técnico** — `coberturas_item_tecnico` ([items_tecnicos.py:174](../../../BDDAT/app/models/items_tecnicos.py)),
  **ya construida en #594**, Carlos no lo sabía y proponía crear
  `proyectos_requisito` nueva — no hace falta. Máquina de 3 estados con dos
  columnas (`texto`+`cubierto`): sin fila o `texto=''` → no revisado;
  `texto=X,cubierto=False` → desfavorable (X justifica el defecto);
  `texto=X,cubierto=True` → favorable (X es la ubicación). Un mismo campo
  `texto` sirve para justificar y para ubicar, según el veredicto — más
  compacto que check+comentario separados. Falta la ruta de tramitador
  (UPSERT) — solo existe el CRUD de Supervisor sobre el catálogo
  (`items_tecnicos/routes.py`).
- **General/directo** — `requerimientos_tarea` (#405). Único de los tres con
  patrón distinto: hay alta/baja real de filas (no veredicto sobre catálogo
  fijo), así que se persiste en bloque (POST único que sustituye la lista),
  no por fila.

`consolidar_defectos()` no es un paso de memoria→disco: lee las tres tablas
ya persistidas (cada una guardada en su propio momento) y congela esa
lectura en `Diagnostico.defectos`. Es una fotografía, no la única escritura.

Relacionado: [[project_backend_solido_revamping]].
