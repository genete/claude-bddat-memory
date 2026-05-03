---
name: Formato de commits BDDAT
description: El proyecto usa [CATEGORÍA] #N descripción, NO conventional commits (feat:/fix:/docs:)
type: feedback
originSessionId: e053ac49-55a3-4e9c-9bea-e1ec52314d38
---
Usar siempre el formato del proyecto: `[CATEGORÍA] #N descripción en imperativo`

Categorías definidas en REGLAS_DESARROLLO.md: `[FEAT]`, `[FIX]`, `[REFACTOR]`, `[DOCS]`, `[TEST]`, `[MERGE]`, `[RELEASE]`, etc.

**Why:** Se usó `feat(#347):` y `docs:` (conventional commits) en lugar de `[FEAT] #347` y `[DOCS]`. El proyecto tiene su propio formato y no usa conventional commits.

**How to apply:** Antes de redactar cualquier mensaje de commit, consultar la sección §Commits de REGLAS_DESARROLLO.md. Sin número de issue cuando el commit no resuelve un issue concreto (ej. `[DOCS] corregir referencias INFORME_AAPP_AAC`).
