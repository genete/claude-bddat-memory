---
name: Workflow NotebookLM para extracción normativa
description: Cómo usar NotebookLM junto con Claude para peinar normas AT de forma eficiente
type: project
---

## Contexto

NotebookLM (cuenta Google independiente) actúa como analizador de normas legales.
Es más preciso que Gemini directo porque trabaja sobre fuentes acotadas sin alucinar.
Se usa para extraer reglas, plazos y variables de normas siguiendo el esquema de GUIA_NORMAS.md.

## Infraestructura Drive

- Unidad Drive montada en `H:\Mi unidad\`
- Carpeta de trabajo: `H:\Mi unidad\bddat-notebooklm\`
- Contenido: ficheros `.txt` (normas individuales) + `contexto_bddat.txt`
- Límite cuenta gratuita: **20 fuentes por cuaderno**

## Generación de ficheros para NotebookLM

### Normas individuales (`.txt`)
```bash
python scripts/legalize_compile.py --solo-confirmadas --individual --out "H:/Mi unidad/bddat-notebooklm/"
```
Genera un `.txt` por norma con cabecera YAML normalizada.
Solo incluye normas con `ambito` asignado y estado no OBSOLETA.

### Contexto BDDAT
```bash
python scripts/preparar_contexto.py --out "H:/Mi unidad/bddat-notebooklm/contexto_bddat.txt"
```

### Normas sedeboja (andaluzas)
Extraer primero con `sedeboja_extract.py --guardar` → quedan en `docs/normas/` → el compile las recoge.

## Prompt estándar para peinar una norma

```
Analiza el documento {ID} ({nombre}) desde la perspectiva de un sistema de
tramitación de expedientes de autorización de instalaciones de alta tensión en Andalucía.

Extrae lo relevante para los procedimientos: AAP, AAC, AE, Transmisión y Cierre.

Para cada regla, documenta en formato lista:
**{ID}-NN** | Descripción | Tipo_solicitud | Fase_afectada | Condición_activación | Excepción_de | Fuente_legal | Notas

Además, en secciones separadas:
1. Plazos — días, silencio administrativo, quién resuelve
2. Variables — parámetros que condicionan el procedimiento
3. Excepciones y regímenes simplificados
```

Nota: pedir lista en lugar de tabla — las tablas de Google Docs no exportan bien a MD.

## Flujo de trabajo incremental

### Sesión tipo
1. NotebookLM analiza la norma → resultado en Google Doc
2. Exportar como MD → pasar a Claude
3. Claude escribe hallazgos en `docs/normas/hallazgos_nblm/` (un fichero por norma)
4. Acumular 4-5 normas antes de volcar a los ficheros de síntesis

### Ficheros de hallazgos
`docs/normas/hallazgos_nblm/{id_tecnico}_reglas.md` — resultado limpio por norma.
No se editan a mano. Son el output de NotebookLM procesado por Claude.

### Ficheros de síntesis (los gordos)
- `docs/NORMATIVA_EXCEPCIONES_AT.md`
- `docs/NORMATIVA_PLAZOS.md`
- `docs/NORMATIVA_MAPA_PROCEDIMENTAL.md`

Se actualizan en lote (4-5 normas de una vez) para minimizar lecturas de ficheros grandes.

### Prompt intermedio (normas posteriores a la primera)
Sustituir el prompt estándar por esta versión ampliada:

```
Analiza el documento {ID} ({nombre}) desde la perspectiva de un sistema de
tramitación de expedientes de autorización de instalaciones de alta tensión en Andalucía.

Extrae lo relevante para los procedimientos: AAP, AAC, AE, Transmisión y Cierre.

Para cada regla, documenta en formato tabla:
**{ID}-NN**; Descripción; Tipo_solicitud; Fase_afectada; Condición_activación; Excepción_de; Fuente_legal; Notas

(Decir "formato tabla" pero dar las cabeceras separadas por ; — así NotebookLM genera tabla correctamente sin confundir | con Markdown)

En las fuentes tienes los ficheros de hallazgos de las normas ya analizadas.
Para cada sección, aplica las siguientes instrucciones:

**Sección Reglas del Motor:**
- Si una regla de esta norma cubre el mismo supuesto que una ya documentada (p.ej. LSE-05),
  indica en las Notas: "Complementa/Supera a [NORMA-NN]".
- Si una regla nueva deroga o restringe una anterior, indica: "Prevalece sobre [NORMA-NN]
  por [rango jerárquico / fecha]" o al revés.
- Solo omite una regla si cubre exactamente el mismo supuesto. Si añade condiciones,
  plazos o matices procedimentales propios, inclúyela aunque referencie la norma anterior.
  En ese caso indica "Cubierta por [NORMA-NN]" solo si es idéntica; si amplía, usa "Complementa a".

**Sección Variables:**
- Para cada variable, documenta en formato tabla:
  Variable; Tipo; Naturaleza (dato/calculado/derivado_doc); Estado; Norma de origen
- Comprueba si ya aparece en los hallazgos anteriores.
- En Estado indica: "Ya existe (ver [NORMA-NN])" si ya estaba documentada,
  "Nueva" si es realmente nueva, o "Renombrar: coincide con [nombre_existente]" si hay
  solapamiento semántico con nombre distinto.

**Sección Excepciones:**
- Si la excepción ya fue documentada en otra norma, indica de qué norma proviene y si
  esta norma la amplía, restringe o confirma.

**Sección 4 — Contradicciones o complementos (OBLIGATORIA):**
- Lista explícita de cada punto de contacto con normas ya analizadas.
- Cita siempre la regla concreta de la norma anterior (p.ej. LSE-07, RD1955-03) y
  el artículo exacto de esta norma.
- Para cada punto indica: Contradicción / Complemento / Prevalencia y quién gana.

No incluyas párrafo introductorio. Empieza directamente con la sección de reglas.
```

**Nota:** el prompt estándar (sin cruce) solo sirve para la primera norma analizada.

## Normas procesadas
| Norma | ID | Estado NotebookLM | Fichero hallazgos |
|---|---|---|---|
| LSE 24/2013 | BOE-A-2013-13645 | Hallazgos escritos | `BOE-A-2013-13645_reglas.md` |
| RD 1955/2000 | BOE-A-2000-24019 | Hallazgos escritos (v2 + revisión manual) | `BOE-A-2000-24019_reglas.md` |
| LPACAP Ley 39/2015 | BOE-A-2015-10565 | Hallazgos escritos | `BOE-A-2015-10565_reglas.md` |
| Ley 21/2013 EIA | BOE-A-2013-12913 | Hallazgos escritos | `BOE-A-2013-12913_reglas.md` |

**Why:** Ahorrar tokens evitando re-explicar el workflow cada sesión y reducir relecturas de ficheros grandes.
**How to apply:** Al iniciar trabajo normativo, leer esta memoria y `docs/normas/hallazgos_nblm/` en lugar de los ficheros de síntesis completos.
