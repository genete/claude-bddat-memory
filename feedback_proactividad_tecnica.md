---
name: Proactividad técnica — inferir objetivo real y ofrecer alternativas
description: Carlos espera que Claude piense antes de ejecutar literalmente, ofrezca alternativas técnicamente superiores y razone sobre el objetivo real detrás de lo que pide.
type: feedback
---

Cuando el usuario pida algo con una solución obvia pero subóptima, no ejecutarla sin más — pensar en el objetivo real y ofrecer alternativas antes.

Ejemplo concreto que originó esta regla: se pidió "extraer datos de una página web del calendario de días inhábiles". La respuesta literal fue buscar en BOE/BOJA y extraer manualmente. La respuesta correcta habría sido inspeccionar primero si la página tenía una API pública subyacente (la tenía: `ssdigitales/datasets/work-calendar/`), que es la fuente programable, automática y más fiable.

**Why:** Carlos quiere que Claude actúe como un técnico que entiende el problema de fondo, no como un ejecutor de instrucciones literales. Los datos del BOE/BOJA son correctos pero no programables; la API es la solución real.

**How to apply:**
- Ante peticiones de extracción de datos web: proponer primero inspeccionar la API subyacente (XHR/fetch en el DOM) antes de hacer scraping o extracción manual.
- Ante cualquier petición: identificar el objetivo real, evaluar si el método pedido es el mejor, y ofrecer alternativas técnicamente superiores antes de ejecutar.
- Si el método literal es claramente inferior, decirlo explícitamente y proponer el mejor antes de pedir confirmación.
- No preguntar "¿quieres que haga X?" si X es lo que el usuario acaba de pedir — añadir valor proponiendo mejoras.
