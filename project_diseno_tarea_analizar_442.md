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

Relacionado: [[project_backend_solido_revamping]].
