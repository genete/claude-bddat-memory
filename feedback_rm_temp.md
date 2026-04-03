---
name: No usar rm en ficheros temporales
description: rm sobre docs_prueba/temp/ siempre queda bloqueado por permisos — usar mv a trash/
type: feedback
---

Nunca intentar borrar ni mover ficheros de `docs_prueba/temp/`. Tanto `rm` como `mv` quedan bloqueados sistemáticamente por el sistema de permisos.

**Why:** El usuario los borra manualmente cuando quiere. La carpeta está gitignored, así que no hay coste en dejarlos indefinidamente.

**How to apply:** Tras escribir y usar un fichero temporal, simplemente no hacer nada más con él. No intentar borrarlo ni moverlo.
