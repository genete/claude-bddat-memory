---
name: Workflow NotebookLM para extracción normativa
description: Cómo usar NotebookLM junto con Claude para peinar normas AT de forma eficiente
type: project
originSessionId: abb34d36-a7fe-4ed3-a093-030aef0ae2f3
---
## Referencias canónicas

- **Prompts y convenciones:** `docs/normas/PROMPT_NBLM.md` — fuente de verdad del prompt. No duplicar aquí.
- **Script consolidado:** `scripts/compile_hallazgos.py` — regenera `hallazgos_consolidados.txt` para el cuaderno.
- **Script contexto:** `scripts/preparar_contexto_nblm.py` — regenera `contexto_normativo_nblm.txt`.

## Infraestructura Drive

- Unidad Drive montada en `H:\Mi unidad\`
- Carpeta de trabajo: `H:\Mi unidad\bddat-notebooklm\`
- Fuentes del cuaderno: `hallazgos_consolidados.txt` + `contexto_normativo_nblm.txt` + norma a analizar
- Límite cuenta gratuita: **20 fuentes por cuaderno**

## Comandos de sesión

```bash
# Antes de analizar una norma nueva:
python scripts/compile_hallazgos.py --out "H:/Mi unidad/bddat-notebooklm/hallazgos_consolidados.txt"

# Copiar norma a analizar desde legalize-es (ejemplo LPACAP):
cp "D:\legalize-es\es\BOE-A-XXXX-XXXXX.md" "H:\Mi unidad\bddat-notebooklm\BOE-A-XXXX-XXXXX.txt"
```

## Flujo de trabajo

1. Regenerar `hallazgos_consolidados.txt` con `compile_hallazgos.py`
2. Subir norma a analizar al cuaderno como `.txt`
3. Lanzar prompt desde `docs/normas/PROMPT_NBLM.md` (estándar o intermedio)
4. Exportar resultado de Google Doc como MD → pasar a Claude
5. Claude escribe `docs/normas/hallazgos_nblm/{BOE-ID}_reglas.md` y hace commit
6. Acumular 4-5 normas antes de volcar a los ficheros de síntesis
7. **Cruce regla a regla** — antes de cerrar la sesión, comparar cada regla del hallazgo contra los docs de síntesis y transferir las que falten. No postponer: la sinergia se pierde y habría que repeinarlo todo.

## Normas procesadas

| Norma | ID | Fichero hallazgos |
|---|---|---|
| LPACAP — Ley 39/2015 | BOE-A-2015-10565 | `BOE-A-2015-10565_reglas.md` |
| LSE — Ley 24/2013 | BOE-A-2013-13645 | `BOE-A-2013-13645_reglas.md` |
| RD 1955/2000 | BOE-A-2000-24019 | `BOE-A-2000-24019_reglas.md` |
| Ley 21/2013 EIA | BOE-A-2013-12913 | `BOE-A-2013-12913_reglas.md` |
| Decreto 9/2011 | sedeboja_22168 | `sedeboja_22168_reglas.md` |
| Decreto-ley 2/2018 | sedeboja_26974 | `sedeboja_26974_reglas.md` |
| RD 337/2014 (RAT) | BOE-A-2014-6084 | `BOE-A-2014-6084_reglas.md` |
| RD 223/2008 (LAT) | BOE-A-2008-5269 | `BOE-A-2008-5269_reglas.md` |

**Why:** El prompt vive en git (`PROMPT_NBLM.md`) para tener historial de cambios. La memoria solo apunta a los ficheros canónicos — no duplica contenido.
**How to apply:** Al iniciar trabajo normativo, leer `docs/normas/PROMPT_NBLM.md` en lugar de esta memoria para el prompt exacto.

## Pendiente: refactor de docs de síntesis (tras terminar MAPEO_CONTEXTO)

Quedan 5 normas en estado `MAPEO_CONTEXTO` sin hallazgos_nblm. Cuando estén todas procesadas, refactorizar los tres MD de síntesis (`NORMATIVA_EXCEPCIONES_AT`, `NORMATIVA_PLAZOS`, `NORMATIVA_MAPA_PROCEDIMENTAL`).

**Problema actual:** los docs de síntesis mezclan referencia normativa completa (que ya está en los hallazgos atómicos) con decisiones de diseño del motor. Crecen sin fin y superan el límite de lectura de Claude (10k tokens).

**Dirección del refactor:**
- `*_reglas.md` = fuente de verdad normativa (ya bien diseñados)
- Docs de síntesis = **solo decisiones de diseño no derivables automáticamente** (resolución de contradicciones, nombres de variables, qué fase del motor activa cada regla)
- El sitio natural para esas decisiones es `DISEÑO_CONTEXT_ASSEMBLER.md`
- Los tres docs actuales podrían reducirse a índices con pointers a hallazgos + notas de decisión

**Why:** Los docs actuales tienen rendimientos decrecientes — el coste de mantenerlos crece más rápido que el valor que aportan.
**How to apply:** Preguntar al usuario por este refactor al inicio de la próxima sesión normativa.
