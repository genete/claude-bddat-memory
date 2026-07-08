---
name: project-plan-estrategia-mapa-necesidades
description: "PLAN_ESTRATEGIA.md ya contiene el mapa de necesidades (§D rol, §G milestone) y una filosofía de issues mínimos (§H) que se ha abandonado"
metadata: 
  node_type: memory
  type: project
  originSessionId: 1d33670c-ad89-449a-913a-badc447875c7
---

`docs/referencia/PLAN_ESTRATEGIA.md` (visión estable, no cambia al cerrar issues) resuelve más de lo que parecía a primera vista al releerlo completo (2026-07-08):

- **§D** — Tabla 14 bloques funcionales × 4 actores (Admin BDDAT/Supervisor/Tramitador/Administrativo), con la interfaz/flujo esperado en cada celda. Es la matriz de necesidades por rol — ver [[project_clasificacion_issues_4bloques]].
- **§G** — Clasifica esos mismos 14 bloques en M1 (bloqueante)/M2 (necesario)/M3+M5 (post-producción)/M4 (pre-producción técnica, no funcional)/opcional, con criterio explícito ("qué pasa si se lanza a producción sin ese bloque"). **Confirma que los milestones actuales siguen siendo la clasificación válida de necesidades** — no hace falta un esquema nuevo, solo cruzar §D/§G con el estado real de los issues.
- La "frontera permeable" M2↔M3 percibida en el día a día está documentada a propósito en §G: motor de reglas/plazos no bloquean el arranque, pero su *estudio arquitectónico* sí es previo ("estudiar ≠ implementar") porque el diseño de tramitación/documental (M1) debe ser compatible con el motor futuro.
- **§H — hallazgo importante:** documenta una filosofía original de "issues mínimos" (2-3 activos simultáneamente, creados solo cuando se va a implementar en días, nunca "para el futuro" ni encadenados) y justifica ahí mismo **por qué se descartó GitHub Projects/Kanban** ("con issues mínimos, un tablero estaría casi siempre vacío"). Hoy hay 96 issues abiertos en 4 milestones — la premisa bajo la que se rechazó Projects ya no se sostiene, y esa decisión nunca se ha vuelto a evaluar explícitamente. Candidato a causa parcial del "efecto hidra" que describe Carlos, distinto de (pero complementario a) la falta de matriz de cobertura.

Ver [[project_adr030_coverage_matrix_pattern]] y el PRE-ADR
`docs/diseño/PRE-ADR-matriz-cobertura-roles-motor.md` (candidato a ADR-031), que ya incorpora estos tres puntos.
