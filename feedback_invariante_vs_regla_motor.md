---
name: feedback_invariante_vs_regla_motor
description: "Antes de implementar cualquier salvaguarda, preguntarse explícitamente si es invariante (hardcode) o modelo legal (reglas_motor) y decirlo"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 6518628d-4a99-456c-a4ce-001a8b2d49a8
  modified: 2026-07-29T18:53:15.162Z
---

Antes de implementar cualquier salvaguarda o bloqueo, hacerse **sistemáticamente** estas dos
preguntas y responderlas en la propuesta, sin esperar a que Carlos las plantee:

1. **El usuario puede hacer lo que sea en cada momento.** Las salvaguardas existen para que se
   haga como se debe hacer, no para impedir. Regla general del proyecto: *todo está permitido
   mientras no esté expresamente prohibido*. Prohibir de forma dura es la excepción que hay que
   justificar.
2. **¿Estoy hardcodeando algo que reglas_motor podría resolver, o es de su dominio?**
   - **Modelo legal** (lo que manda una norma y puede cambiar con ella) → `reglas_motor`, para
     poder responder a un cambio legal sin reprogramar. Es una premisa del proyecto.
   - **Invariante estructural** (es así porque se hace así; coherencia de datos o de evidencia)
     → hardcode. **Por defecto `puede_escapar: true`**: obligar a justificar ya hace que el
     usuario se pare, y eso basta casi siempre. `puede_escapar: false` es la **excepción**, y
     hay que argumentarla caso a caso.
   - **Dato de catálogo** (qué tipos/códigos participan) → tabla, no constante en código. Una
     lista de códigos dentro de un módulo de servicio es deuda de dato aunque la regla que la
     usa sea invariante — decirlo aunque no se cambie en ese issue.

**Criterio para cerrar la puerta del todo (`puede_escapar: false`):** que el acto haya salido
fuera y ya no se pueda deshacer — típicamente **notificado** (LPACAP). Mientras todo queda en
casa, hay escape con justificación. Ejemplo canónico, de #714 (reversión del diagnóstico): si el
requerimiento ya se notificó al titular, no hay vía de escape —es evidencia de lo comunicado—;
si no se ha notificado, sí la hay.

**Why:** son dos premisas de arquitectura del proyecto (respuesta rápida a cambios legales sin
tocar código; y no encorsetar al técnico). Confundir las capas produce reglas legales enterradas
en código y bloqueos duros donde bastaba una justificación en bitácora. Carlos corrigió
explícitamente el sesgo contrario: *"prefiero que el «no hay salida» sea la excepción"*.

**How to apply:** al presentar una solución, decir a qué capa pertenece cada pieza y por qué.
El patrón por defecto es *permitir con justificación* → bitácora
(`{'escape': True, 'justificacion': ...}`); cerrar del todo exige justificar por qué el acto ya
no se puede deshacer. Ver [[project_capas_catalogo_motor]] y [[feedback_proactividad_tecnica]].
