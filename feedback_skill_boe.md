---
name: feedback_skill_boe
description: Usar siempre el skill /boe para leer legislación, no WebFetch directamente
type: feedback
---

Cuando hay que leer artículos concretos de legislación (BOE o BOJA), usar siempre el skill `/boe`
en lugar de intentar WebFetch por libre. El skill conoce la diferencia BOE (API REST ELI) vs
legislación andaluza (sedeboja con Playwright + JSF), los IDs conocidos y el flujo correcto.

**Why:** En sesión 2026-04-03, Claude intentó acceder al DL 26/2021 via WebFetch repetidas veces
a URLs incorrectas o sin contenido útil, antes de que el usuario señalara que existía el skill.

**How to apply:** En cuanto aparezca la necesidad de leer un artículo o disposición de una norma,
invocar `Skill("boe", ...)` antes de cualquier intento de WebFetch o navegación manual.
