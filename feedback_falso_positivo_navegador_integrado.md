---
name: feedback_falso_positivo_navegador_integrado
description: "El navegador integrado puede renderizar ciertas islas React con coordenadas/posición incorrectas (ej. panel Inspector de tablas_maestras) sin que sea un bug real — no concluir \"bug confirmado\" solo por reproducirlo ahí"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 09dc9729-d1cc-428d-a697-4451781cbd79
  modified: 2026-08-03T08:07:17.654Z
---

En sesión sobre #725 (2026-08-03), reproduje dos veces vía `mcp__Claude_Browser__*` que el
botón "Editar" del Inspector de `tablas_maestras` (edición de `tramites_tareas`) se renderizaba
con `getBoundingClientRect().x ≈ 2065px` en un viewport de 1280px — fuera de pantalla,
aparentemente inalcanzable. Lo di por "bug confirmado" porque lo verifiqué con JS directo
(no solo con la herramienta de clic), pensando que eso descartaba un artefacto de la
automatización.

Carlos no lo reproduce en su navegador real — es un falso positivo específico del navegador
integrado con esa isla React en concreto, no un defecto de la aplicación.

**Por qué:** confirma en un caso real la excepción que ya anticipaba `CLAUDE.md` §"Verificación
de cambios en navegador" — islas React con comportamiento vivo (rAF, observers, paneles con
transform/posicionamiento dinámico) pueden no reflejarse bien en el navegador integrado. El
panel Inspector de `tablas_maestras` (carga contenido vía fetch/estado y desliza un drawer) es
uno de esos casos, no solo react-flow/Recharts como decía el ejemplo original.

**Cómo aplicar:** verificar con JS (`getBoundingClientRect`) que algo esté "realmente" fuera de
pantalla NO es prueba suficiente de un bug real cuando el elemento vive dentro de un panel
deslizante/animado — ese tipo de componente es precisamente el candidato a comportarse distinto
en el navegador integrado. Antes de reportar un hallazgo de este tipo como "bug confirmado",
contrastarlo con Carlos en vez de darlo por cerrado con una sola reproducción propia,
especialmente si el elemento pertenece a un panel/drawer con posicionamiento dinámico. Ver
[[feedback_navegador_integrado_por_defecto]].
