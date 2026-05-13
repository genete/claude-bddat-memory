---
name: project-catalogo-documentos-alcance
description: "Alcance del catálogo de tipos_documentos: qué cubre y qué queda pendiente"
metadata: 
  node_type: memory
  type: project
  originSessionId: f9e651a8-5655-4d16-a8b3-1cdf8a972115
---

El catálogo sembrado en #337 (53 tipos) cubre los **documentos operacionales del flujo ESFTT**: los que genera o recibe la administración durante la tramitación estándar (ELABORAR, NOTIFICAR, ANALIZAR, ESPERAR_PLAZO).

**Fuera del catálogo actual:** la documentación presentada por el titular en la solicitud — tasas, memoria técnica, planos, solicitud formal, declaraciones responsables, etc. Estos documentos entran al pool del expediente antes de que empiece el flujo de tareas.

**Por qué:** la catalogación de documentación presentada depende de la UI de recepción de documentación o de la lógica del motor que los referencie. No se hace de una vez.

**Patrón establecido:** `DR_NO_DUP` (creado para probar el motor, regla no-IP por Decreto 9/2011 cuando no hay DUP ni AAU) es el modelo a seguir: cada vez que el motor necesite evaluar un tipo nuevo, se añade al catálogo como parte del issue del motor que lo requiera.

**How to apply:** cuando un issue del motor de reglas o plazos necesite un tipo de documento que no existe en el catálogo, añadirlo en la migración de ese issue (como hizo #373 con CERT_FIN_INSTRUCCION). No crear issues separados solo para añadir tipos.
