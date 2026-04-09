---
name: Workflow NotebookLM para extracción normativa
description: Cómo usar NotebookLM junto con Claude para peinar normas AT de forma eficiente
type: project
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

## Normas procesadas

| Norma | ID | Fichero hallazgos |
|---|---|---|
| LPACAP — Ley 39/2015 | BOE-A-2015-10565 | `BOE-A-2015-10565_reglas.md` |
| LSE — Ley 24/2013 | BOE-A-2013-13645 | `BOE-A-2013-13645_reglas.md` |
| RD 1955/2000 | BOE-A-2000-24019 | `BOE-A-2000-24019_reglas.md` |
| Ley 21/2013 EIA | BOE-A-2013-12913 | `BOE-A-2013-12913_reglas.md` |

**Why:** El prompt vive en git (`PROMPT_NBLM.md`) para tener historial de cambios. La memoria solo apunta a los ficheros canónicos — no duplica contenido.
**How to apply:** Al iniciar trabajo normativo, leer `docs/normas/PROMPT_NBLM.md` en lugar de esta memoria para el prompt exacto.
