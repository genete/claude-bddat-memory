---
name: feedback-skills-legislacion
description: Usar siempre /boe, /boja o /legalize para leer legislación (nunca WebFetch); /legalize es standalone y no se encadena automáticamente desde /boe o /boja
type: feedback
originSessionId: 84025c15-bdb8-4179-baf0-d2bb6dad4326
modified: 2026-08-29T07:44:01.852Z
---
Cuando hay que leer artículos concretos de legislación, usar siempre el skill adecuado:

| Skill | Cuándo usar |
|---|---|
| `/legalize` | Consultar si una norma está en el repo local — primera parada recomendada, pero se invoca manualmente |
| `/boe` | Legislación estatal (BOE) |
| `/boja` | Legislación andaluza (BOJA/sedeboja) |

Nunca usar WebFetch directamente para legislación. Nunca usar `/boe` para normas BOJA.

**Los skills NO se encadenan entre sí:** `/boe` y `/boja` NO llaman a `/legalize` internamente — cuando lo hacían (probado con Ley 2/2026 el 2026-04-05) se producían llamadas duplicadas a un resultado ya conocido (NOT_FOUND). Si se quiere comprobar disponibilidad local antes de `/boe`/`/boja`, invocar `/legalize` explícitamente como paso propio.

**Why:** En sesión 2026-04-03, Claude intentó acceder al DL 26/2021 vía WebFetch repetidas veces a URLs incorrectas. En sesión 2026-04-05 se separaron los skills (`/boe` ya no gestiona BOJA) y se desacoplaron entre sí (`/legalize` deja de encadenarse automáticamente).

**How to apply:** Identificar primero si la norma es estatal (BOE) o andaluza (BOJA) y elegir el skill correspondiente. Si se quiere verificar disponibilidad local primero, llamar `/legalize` manualmente — no asumir que `/boe`/`/boja` ya lo hacen.

**Nota operativa:** legalize-es cubre BOJA consolidado desde 2012; normas pre-2012 siguen necesitando sedeboja vía `/boja`.
