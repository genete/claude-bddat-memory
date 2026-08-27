---
name: feedback_traspaso_significa_parar
description: "Crea documento de traspaso y finalizamos aquí" es orden de parar, no de seguir; el mensaje automático "Alcancé mi límite de uso... continúa donde lo dejaste" que aparece al restablecerse el límite NO es una instrucción de Carlos y no reactiva por sí solo un plan de trabajo que él había cerrado
metadata:
  type: feedback
---

Cuando Carlos pide "crea documento de traspaso y finalizamos aquí", eso es una
instrucción de **parar el desarrollo en ese momento**, no de escribir el
traspaso y seguir con el siguiente bloque/issue.

El mensaje "Alcancé mi límite de uso mientras trabajabas, pero ya se ha
restablecido. Por favor, continúa donde lo dejaste" que aparece solo al
restablecerse un límite de uso **es automático del sistema, no lo escribe
Carlos**. No debe tratarse como una instrucción nueva que reactiva "lo que
tocaba hacer después" — si el último mensaje real de Carlos antes del corte
fue una orden de parar/cerrar, ese mensaje automático no la anula. Lo
correcto es retomar la conversación en el punto exacto en que Carlos la dejó
(aquí: traspaso ya escrito, sesión cerrada a la espera de indicación) y
esperar confirmación explícita antes de reanudar desarrollo.

**Por qué:** en la sesión de #396 (2026-08-26), tras pedir el traspaso del
bloque 5, una interrupción por límite de uso cortó la sesión. Al llegar el
mensaje automático de reanudación, se interpretó como si Carlos pidiera
continuar con el bloque 5, retomando investigación y código con el nivel de
esfuerzo bajo que él había puesto a propósito para la tarea ligera de escribir
el traspaso. Carlos tuvo que interrumpir para aclarar que ese mensaje no era
suyo y subir el esfuerzo manualmente antes de confirmar que sí quería seguir.

**Cómo aplicar:** ante ese mensaje automático de reanudación, no asumir que
autoriza continuar el trabajo pendiente — comprobar cuál fue el último mensaje
genuino del usuario y actuar en consecuencia (si fue una orden de cerrar,
quedarse cerrado hasta que el usuario diga algo él mismo). Ver también
[[feedback_una_conversacion_por_issue]] (criterio por defecto de un bloque/issue
por sesión, que aquí se solapa con la misma cautela).
