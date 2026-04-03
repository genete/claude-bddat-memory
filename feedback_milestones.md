---
name: Criterio de milestones por dependencia entre issues
description: Al sugerir milestones, issues enlazados deben estar en el mismo milestone que su dependencia
type: feedback
---

Al sugerir en qué milestone ubicar un issue, si ese issue está relacionado/enlazado con otro que ya tiene milestone asignado, situarlo en el mismo milestone que su dependencia.

**Why:** El issue #285 (diagrama interactivo ESFTT) fue sugerido para M5 por ser una feature visual avanzada, pero el usuario lo corrigió a M2 porque depende del #274, que ya estaba en M2. La relación entre issues tiene más peso que la percepción de "complejidad" de la feature.

**How to apply:** Antes de sugerir milestone, revisar si el issue referencia a otros (`closes #X`, `relacionado con #X`, etc.) y consultar el milestone de esos issues primero.
