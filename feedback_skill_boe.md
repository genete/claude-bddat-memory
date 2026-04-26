---
name: feedback_skill_boe
description: Usar siempre los skills /boe, /boja o /legalize para leer legislación, no WebFetch directamente
type: feedback
originSessionId: 84025c15-bdb8-4179-baf0-d2bb6dad4326
---
Cuando hay que leer artículos concretos de legislación, usar siempre el skill adecuado:

| Skill | Cuándo usar |
|---|---|
| `/legalize` | Consultar si una norma está en el repo local (primera parada automática de los otros) |
| `/boe` | Legislación estatal (BOE) |
| `/boja` | Legislación andaluza (BOJA/sedeboja) |

Nunca usar WebFetch directamente para legislación. Nunca usar `/boe` para normas BOJA.

**Why:** En sesión 2026-04-03, Claude intentó acceder al DL 26/2021 via WebFetch repetidas veces
a URLs incorrectas. En sesión 2026-04-05 se separaron los skills: `/boe` ya no gestiona BOJA.

**How to apply:** Identificar primero si la norma es estatal (BOE) o andaluza (BOJA) y elegir
el skill correspondiente. Para verificar disponibilidad local, llamar `/legalize` manualmente antes de `/boe` o `/boja`. Los skills NO se encadenan automáticamente entre sí.

**Nota operativa:** legalize-es cubre BOJA consolidado desde 2012; normas pre-2012 siguen necesitando sedeboja vía `/boja`.
