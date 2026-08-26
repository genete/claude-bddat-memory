---
name: feedback-partir-de-la-norma-no-de-la-implementacion
description: "En cuestiones jurídicas, derivar de la norma leída (/boe, /boja), no de lo que ya afirman el código y los docs de diseño; estos arrastran premisas falsas"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: db4fa4f1-9ef7-430b-951d-b98ab064b061
  modified: 2026-08-18T08:25:19.814Z
---

Ante una pregunta de fondo jurídico (¿de quién es este plazo? ¿qué lo suspende?
¿desde cuándo cuenta?), leer **la norma** antes de razonar. No reconstruir la
respuesta a partir de `DISEÑO_*.md`, de los seeds de migración o del código
existente: esos derivan de lecturas anteriores y pueden arrastrar una premisa
equivocada que se hereda sin darse cuenta.

**Why:** en la sesión de #788 (2026-08-17) heredé de `DISEÑO_FECHAS_PLAZOS.md` y
de `catalogo_plazos` la premisa de que las Fases tienen plazo propio, y la sostuve
tres turnos seguidos pese a las correcciones de Carlos. Era falsa: solo Solicitud y
Tarea portan fecha administrativa, así que solo ellas pueden ser sujeto de un
plazo; Fase y Trámite son taxonomía ESFTT, no figuras jurídicas. Al leer por fin el
art. 22 LPACAP literal, se resolvieron de golpe tres cuestiones que llevaban
turnos abiertas (objeto único de la suspensión, cómputo de extremos `B − A`, y una
cita mal atribuida en el propio documento de diseño).

**How to apply:** cuando la pregunta sea "¿qué dice la ley?", invocar `/boe` o
`/boja` antes de proponer diseño, no después de que el usuario lo pida. Los
documentos de diseño del repo son consumidores de la norma, no fuente: si
contradicen el texto legal, se corrigen ellos. Y si el usuario corrige una premisa,
revisar si esa misma premisa contamina otras partes del análisis en curso — en
#788, caer FASE implicaba caer TRÁMITE, y no lo vi hasta que me lo señalaron.

Relacionado: [[feedback_skill_boe]], [[feedback_verificar_con_datos_reales_e_historial]],
[[feedback_vigencia_modificaciones_normativas]]
