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
Añadir al prompt estándar:
> "En las fuentes tienes los hallazgos consolidados de las normas ya analizadas.
> Tenlos en cuenta para evitar duplicar reglas ya documentadas e identificar
> contradicciones o complementos."

## Normas procesadas
| Norma | ID | Estado NotebookLM |
|---|---|---|
| LSE 24/2013 | BOE-A-2013-13645 | Hallazgos pendientes de escribir |

**Why:** Ahorrar tokens evitando re-explicar el workflow cada sesión y reducir relecturas de ficheros grandes.
**How to apply:** Al iniciar trabajo normativo, leer esta memoria y `docs/normas/hallazgos_nblm/` en lugar de los ficheros de síntesis completos.
