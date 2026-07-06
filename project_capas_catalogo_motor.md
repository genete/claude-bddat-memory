---
name: capas-catalogo-motor
description: Arquitectura en tres capas de tipos_fases/tramites/tareas y motor de reglas — qué necesita programador y qué no
metadata: 
  node_type: memory
  type: project
  originSessionId: 0accc5a9-30da-4bc0-bbbc-45a620167ffa
---

BDDAT separa en tres capas independientes lo que a primera vista parece una sola tabla de catálogo (`tipos_fases`, `tipos_tramites`, `tipos_tareas`):

1. **Vocabulario** (la fila en la tabla) — genérico, sin lista blanca en código. `app/services/tipos_creables.py` itera `TipoFase.query.order_by(...).all()` completo para poblar las opciones del árbol; `mutaciones_arbol.crear_fase()` acepta cualquier `tipo_fase`. Una fila nueva es visible y creable sin tocar código.
2. **Reglas** (`reglas_motor` + `condiciones_regla` + `excepciones_motor`) — ya 100% dato, evaluadas por `_evaluar()`/`build_sujeto()` en cada mutación (`crear_fase`, `crear_tramite`...). Es la capa que las UIs pendientes #170/#171 expondrían al Supervisor sin programador.
3. **Casos especiales** (Python hardcodeado: `_FASES_QUE_REQUIEREN_CERT_IP_CONSULTAS`, ramas por `codigo ==` en `calculado.py`/`estado_dominio.py`) — esto sí requiere programador siempre, porque es comportamiento nuevo, no solo dato nuevo.

`docs/referencia/ESTRUCTURA_ESF.json` (qué fases corresponden a qué tipo_solicitud, con sus condiciones legales) hoy NO lo lee ningún servicio — es documentación para humanos. Su propia cabecera dice que esas restricciones deben migrar a `reglas_motor` (ADR-007): confirma que la capa 2 es el destino de diseño correcto, no una tabla nueva.

**Cómo aplicar:** ante "¿puede el Supervisor crear un tipo_X nuevo sin programador?", la respuesta depende de en qué capa cae lo pedido — vocabulario y reglas ya lo permiten (o lo permitirían con UI), casos especiales no. Ver también `app/checks/catalogo_requerido.py` (#347): manifiesto de qué códigos concretos de estas tablas están anclados en la capa 3.

**Generalización del Variable Registry — evaluada y descartada (2026-07-04).** Se planteó permitir que el Supervisor cree variables tipo "dato" (passthrough simple de un campo existente, ver `app/services/variables/dato.py`) mediante una ruta de atributo en texto en vez de una función Python. Decisión: no perseguir. El ahorro es modesto (solo cubre "el dato ya existe, falta exponerlo") y el coste es real: una ruta de atributo en texto no la detecta ningún refactor/grep, a diferencia de la función Python actual — cambiaría "el programador escribe 3 líneas" por "acoplamiento invisible al refactor". Si resurge la idea, partir de este análisis en vez de repetirlo.
