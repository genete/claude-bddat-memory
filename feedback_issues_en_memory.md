---
name: feedback_issues_en_memory
description: No guardar en memoria el estado de issues, ramas activas ni "por dónde vamos" — eso pertenece a GitHub y git
type: feedback
originSessionId: 84025c15-bdb8-4179-baf0-d2bb6dad4326
---
No meter en memoria referencias a issues (#NNN), estado de ramas, ni resúmenes de "qué está activo ahora". Esa información es derivable de GitHub y de `git branch`/`git log`.

**Why:** Claude tiende a equiparar "estado del proyecto" con "contexto útil para próximas sesiones" y lo vuelca en memoria. Pero el estado de los issues vive en GitHub; la rama activa, en git. La memoria debe contener solo lo que NO se puede derivar de esas fuentes: decisiones de diseño no escritas en el código, preferencias de trabajo, reglas de comportamiento.

**How to apply:** Antes de añadir cualquier referencia a un issue o rama en memoria, preguntar: ¿puede un Claude futuro encontrar esto en GitHub o git? Si sí → no guardarlo.
