---
name: verificar-atribucion-codigo
description: "Al mapear dependencias código→catálogo por grep, confirmar la clase real detrás de un .codigo antes de atribuirlo a una tabla"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 0accc5a9-30da-4bc0-bbbc-45a620167ffa
---

Al auditar qué filas de catálogo dependen de código hardcodeado (comparaciones tipo `algo.codigo == 'X'`), no asumir el modelo/tabla por similitud de nombre de campo. Un `.codigo` puede pertenecer a un dataclass interno sin tabla detrás (p.ej. `EstadoPista.codigo` en `app/services/seguimiento.py`, cuyo vocabulario vive en los dicts `COLOR`/`PRIORIDAD` de `estado_dominio.py`), no a un modelo SQLAlchemy real.

**Por qué:** en la auditoría de `app/checks/catalogo_requerido.py` (2026-07-04) atribuí `'FIN'` a `TipoResultadoFase` solo por el patrón textual `e.codigo == 'FIN'`, sin rastrear qué era realmente `e`. Era un falso positivo — lo detectó ejecutar `validar_catalogo()` de verdad contra la BD real (petición explícita del usuario), no el grep ni los tests mockeados (que por construcción no pueden atrapar este tipo de error). Ver [[project_capas_catalogo_motor]].

**Cómo aplicar:** antes de dar por buena una atribución código→tabla basada en grep, rastrear hacia atrás de dónde sale la variable/objeto (¿instancia de un modelo, o de un dataclass/proyección interna?). Si hay duda y la conclusión va a escribirse en una fuente de verdad (manifiestos, migraciones), verificar ejecutando el chequeo contra datos reales antes de darlo como definitivo.
