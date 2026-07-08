---
name: project-adr030-coverage-matrix-pattern
description: Patrón metodológico de ADR-030 (matriz dimensión×valor con placeholders explícitos) y su aplicación más allá de tests/seed
metadata: 
  node_type: memory
  type: project
  originSessionId: 1d33670c-ad89-449a-913a-badc447875c7
---

ADR-030 (`docs/decisiones/ADR-030-dataset-ficticio-y-matriz-cobertura.md`, adoptada 2026-07-07) diseñó el dataset ficticio estable y la matriz de cobertura de pre-producción para motor de reglas, plazos, Variable Registry, invariantes estructurales y generación de documentos.

**El principio reutilizable (§9):** para cada dimensión relevante (cada operador, cada efecto, cada unidad de plazo, cada variable...) debe existir un lugar reservado en la matriz, aunque la celda concreta quede como placeholder documentado (`pytest.skip('razón — #issue futuro')`) en vez de resuelta. Hacer el análisis de solo una parte del mapa y dejar el resto sin identificar se considera **peor** que dejar huecos explícitos y señalizados. Criterio de parada: un caso por cada valor de cada dimensión, no el producto cartesiano completo.

**Por qué importa fuera de ADR-030:** este mismo patrón es el que le falta a Carlos para responder "¿qué necesita cada rol (supervisor/tramitador/administrativo/admin) que aún no tiene issue?" y "¿qué parte del core motor está incompleta?" — ver [[project_clasificacion_issues_4bloques]]. Un issue tracker (por bien clasificado que esté) solo puede mostrar huecos que ya se convirtieron en issue; una matriz de cobertura por rol/área, al estilo ADR-030, expondría huecos que hoy son invisibles porque nadie los ha filed todavía. Es un ejercicio de auditoría explícito y separado (enumerar dimensiones + marcar cobertura), no una reclasificación de lo existente — pendiente de acometer en sesión propia.
