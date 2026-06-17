---
name: feedback_consolidar_antes_de_nuevo
description: Priorizar consolidar la infraestructura y migrar lo existente antes de construir vistas nuevas aisladas
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 5e241f41-61fb-4851-9aac-2090b51adb8e
---

Ante el orden del roadmap, Carlos prioriza: (1) terminar la infraestructura, (2) adaptar/migrar lo existente con ella —capturando la lección—, y (3) solo entonces construir las vistas nuevas aisladas, ya con el patrón maduro. Rechaza adelantar una vista nueva e independiente (p. ej. #501 "Mi trabajo") por delante de la migración de los listados existentes, y distingue lo **preceptivo** de lo meramente importante (p. ej. #532 frontend palette es importante pero no prerequisito de la infra del inspector).

**Why:** construir lo nuevo con un patrón probado en un solo caso arriesga re-trabajo y deja la app inconsistente; migrar primero lo existente madura el patrón y da coherencia. Además, una vista que no ejercita la infra nueva (un agregado que navega al árbol, sin inspector) no aporta nada a madurarla si se adelanta.

**How to apply:** al proponer o revisar el orden de implementación, ordenar infra → migración de lo existente → vistas nuevas; marcar explícitamente qué issues son preceptivos de la infra y cuáles son aislados (esos van al final o cuando haya hueco). La única razón legítima para adelantar una vista nueva es presión de negocio real, que decide Carlos. Relacionado: [[feedback_milestones]], [[feedback_analisis_impacto]].
