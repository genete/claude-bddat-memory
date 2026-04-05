---
name: Rediseño motor de reglas — sesión 2026-03-31
description: Decisiones y deudas técnicas sobre el motor de reglas, fechas y issues M2 — sesión de reflexión arquitectural
type: project
---

## Decisión principal: Motor de reglas agnóstico

**Qué se decidió:** El motor de reglas debe refactorizarse para ser completamente agnóstico de BDDAT.
En lugar de que el motor conozca el dominio (Fase, Tarea, Expediente, etc.) y haga queries internos,
el flujo correcto es:

1. **BDDAT** ensambla un dict plano de variables antes de llamar al motor
2. **Motor** recibe `(accion, tipo, variables: dict)` y evalúa reglas almacenadas en BD contra ese dict
3. El motor no importa modelos de BDDAT, no traversa el árbol ESFTT, no hace queries propias

```python
# BDDAT ensambla
variables = {
    'ia': 'AAU',
    'es_modificacion': False,
    'intermunicipal': True,          # BDDAT lo calcula (len(municipios) > 1)
    'fase_ip_cerrada': False,        # BDDAT lo calcula (query a Fase)
    'tiene_doc_producido': True,     # BDDAT lo calcula
    'plazo_vencido': False,          # BDDAT lo calcula (controlador de fechas)
}
# Motor evalúa — sin saber de dónde vienen los valores
motor.evaluar(accion='INICIAR', tipo='TRAMITE', variables=variables)
```

**Por qué ahora y no después:** Las tablas `reglas_motor` y `condiciones_regla` aún no están
pobladas con reglas reales. El coste de refactorizar es mínimo ahora, altísimo después.

**Pieza nueva: ContextAssembler**
BDDAT necesita una capa que, dado un evento + entidad, suba el árbol ESFTT y compute
todas las variables necesarias antes de llamar al motor. Esta capa SÍ conoce BDDAT a fondo.
Su diseño tiene miga propia (ver pendientes).

---

## Problemas que resuelve el motor agnóstico

| Problema con diseño actual | Solución con motor agnóstico |
|---|---|
| `intermunicipal` requería query especial en el motor | ContextAssembler lo computa y pasa como variable |
| `documentos_tarea` (INCORPORAR multi-doc) no accesible al motor | ContextAssembler pasa `tiene_docs_incorporar: True` |
| No existía `VARIABLE_TRAMITE` ni `VARIABLE_FASE` como criterios | Desaparecen — todo son variables del dict |
| `_TIPOS_REQUIEREN_DOC_PRODUCIDO` hardcodeado en motor | ContextAssembler lo computa según tipo de tarea |
| Plazos/fechas inaccesibles al motor | ContextAssembler pasa `plazo_vencido`, `dias_transcurridos`, etc. |
| `solicitud.estado` sin integridad referencial | Pasa como variable; su validez la gestiona BDDAT |

---

## Problemas del diseño actual (deuda técnica confirmada)

### 1. Criterios BDDAT-específicos en el motor (a eliminar)
- `EXISTE_FASE_CERRADA` — el motor conoce el modelo Fase
- `EXISTE_TAREA_TIPO` — el motor conoce el modelo Tarea
- `_check_finalizar_tarea` — hardcoded con lógica de BDDAT
- `EXISTE_DOCUMENTO_TIPO`, `EXISTE_DOC_ORGANISMO` — stubs, pendientes de implementar

### 2. Valores inline sin tabla (riesgo de integridad)
- `Solicitud.estado` — VARCHAR con `EN_TRAMITE|RESUELTA|DESISTIDA|ARCHIVADA`, sin FK
  → Motor compara strings; si cambia en Python, `condiciones_regla` queda huérfano
- `_TIPOS_REQUIEREN_DOC_PRODUCIDO` / `_TIPOS_REQUIEREN_DOC_USADO` — sets hardcodeados
  en `motor_reglas.py`; son propiedades funcionales de `TipoTarea` que deberían estar en BD

### 3. Estados derivados (@property) sin criterio genérico
- `planificada`, `en_curso`, `ejecutada` son @property correctas como contrato del modelo
- Pero el motor no puede referenciarlas en reglas configurables — solo via criterios especializados
- Con motor agnóstico esto deja de ser problema: ContextAssembler las computa y las pasa

---

## Controlador de fechas — pieza no diseñada, bloquea varias cosas

**Estado:** No existe diseño. No hay documento. No hay implementación.

**Qué bloquea:**
- El campo `PLAZO_DIAS`/`TIPO_DIAS` de ESPERAR_PLAZO vive en `tarea.notas` como texto libre
  → inaccesible para el motor y para cualquier cálculo automatizado
- La vista de seguimiento no puede mostrar expedientes vencidos o próximos a vencer
- El motor no puede evaluar condiciones temporales (plazo vencido, días transcurridos)
- #279 (campos extra proyecto) puede incluir datos que afectan a plazos → dependencia

**Preguntas de diseño abiertas (responder antes de implementar):**
1. ¿`PLAZO_DIAS` y `TIPO_DIAS` salen de `tarea.notas` y se convierten en campos reales de Tarea?
2. ¿El motor necesita evaluar "plazo vencido" como condición de BLOQUEAR, o solo es informativo?
3. ¿Fecha de vencimiento se calcula al vuelo o se persiste?
4. ¿El controlador de fechas es parte del ContextAssembler o una capa separada?

---

## ContextAssembler — pendiente de diseño, tiene complejidad propia

**Qué es:** Capa BDDAT que ensambla el dict de variables antes de llamar al motor.

**Complejidad pendiente:**
- ¿Qué variables computa para cada (accion, tipo)? ¿Es hardcoded o configurable en BD?
- El Supervisor debe poder editar acciones, variables y condiciones → hay que conjugar
  flexibilidad con que no todo sea personalizable
- Variables derivadas (intermunicipal, plazo_vencido, tiene_fase_X_cerrada) requieren
  queries específicas — ¿cómo se declaran sin hardcodearlas?
- El contrato entre ContextAssembler y Motor define qué variables existen →
  si es configurable, el Supervisor necesita UI para declarar variables nuevas

**Principio acordado:** Separar responsabilidades, no crear la panacea.
El motor es agnóstico. BDDAT se adapta al API del motor, no al revés.

---

## Issues M2 afectados y estado tras la sesión

### #290 — INCORPORAR multi-doc (tabla documentos_tarea)
**Estado: EN STANDBY** hasta resolver el diseño del controlador de fechas.
**Por qué:** La validación de FINALIZAR para INCORPORAR pasará del motor actual
(`doc_producido_id IS NULL → BLOQUEAR`) al ContextAssembler (`tiene_docs_incorporar`).
Implementar #290 ahora requeriría cambiar esa lógica dos veces.
**Nota técnica:** El cambio en el motor es puntual (solo `_check_finalizar_tarea`), pero
con el rediseño agnóstico desaparece del motor y pasa al ContextAssembler.

### #279 — Campos extra en Proyecto (tecnología, kV, kVA, tipo suelo...)
**Estado: REQUIERE DISEÑO PREVIO** — no implementar hasta responder:
- ¿Qué campos van en `Proyecto` y cuáles en tabla catálogo propia?
- ¿Tipo de suelo es enum o tabla `tipos_suelo`?
- ¿Tensión/potencia son numéricos libres o rangos normalizados?
- ¿Qué reglas del motor los consultan?
**Nota:** Con motor agnóstico, cualquier campo nuevo en `Proyecto` es automáticamente
accesible via ContextAssembler → no requiere cambios en el motor.

### #289 — Context Builders (Capa 2 generación escritos)
**Estado: BLOQUEADO** por #279 y por falta de datos reales en expedientes.
Los builders beben de campos extra de proyecto que aún no existen.

### #281, #285 — Refactor vistas / Diagrama Mermaid
**No afectados** por las decisiones de esta sesión. Pueden avanzar independientemente.

---

## Orden de trabajo recomendado (revisado)

1. **Diseño motor agnóstico** — antes de poblar `reglas_motor` con reglas reales
2. **Diseño controlador de fechas** — desbloquea #290 y #279
3. **Diseño ContextAssembler** — qué variables, cómo se declaran, qué configura el Supervisor
4. **#279** — después de los diseños anteriores
5. **#290** — después del controlador de fechas
6. **#289** — después de #279 y con datos reales

---

## Principios de diseño acordados en sesión

- Motor agnóstico: "es el procedimiento el que se engancha al API del motor, no al revés"
- Todo permitido excepto lo expresamente prohibido (ya en diseño)
- Principio de escape: toda cascada y regla tiene vía de escape (ya en diseño)
- Variables del motor deben tener integridad referencial o contrato explícito
- No cubrir todas las reglas legales de golpe — añadir una, probar, iterar

---

## Principios añadidos en sesión 2026-04-05 (análisis LISTA)

### El motor es radicalmente agnóstico — el Supervisor orquesta

El motor lee contexto (dict de variables), evalúa reglas definidas en tabla (texto y números)
y devuelve permitido/prohibido. **No sabe nada del dominio**: ni quién acredita qué, ni si
una variable la aporta el promotor o la calcula BDDAT. Eso es razonamiento del Supervisor.

El Supervisor (nosotros) es quien:
1. Descubre variables y sus combinaciones analizando la normativa.
2. Define las condiciones correctas (una o varias, encadenadas o no) que alimentan las reglas.
3. Garantiza que las reglas no crean estados imposibles de desbloquear.

Las reglas empiezan simples y crecen en complejidad a medida que aparecen nuevos casos.

### El sistema de reglas es caótico — el motor no

Las reglas que el motor lee son un sistema multivariable fácilmente caótico. Fuentes de caos:
- Cambios legislativos → el Supervisor debe actualizar reglas.
- Razonamientos incorrectos previos → reglas que hay que corregir.
- Combinaciones de variables no contempladas → reglas que no cubren el caso.
- Reglas que crean deadlocks (imposibles de desbloquear por combinación entre ellas).

**Consecuencia:** toda regla nueva debe razonarse con cuidado antes de entrar en la tabla.
El Supervisor es la última barrera antes del caos.

### Cuando dos condiciones eximen el mismo hito: evaluar secuencialmente, no como OR

Si hay condición A (exención legal objetiva) y condición B (requisito procedimental cumplido)
que ambas eximen el mismo hito, no son intercambiables. El orden de evaluación importa y
debe estar documentado. El motor las evaluará según cómo estén definidas las reglas —
si se definen como OR, el usuario puede abusar rellenando la condición incorrecta.

### Condiciones que solo el promotor puede acreditar → el motor bloquea, no decide

Cuando la administración no puede evaluar objetivamente una condición (ej: si una
modificación excede el polígono autorizado), el motor debe bloquear hasta que el promotor
aporte la documentación. Nunca asumir en un sentido u otro. Esto se traduce en que
la variable correspondiente se deja vacía/null y el motor la evalúa como bloqueante.
