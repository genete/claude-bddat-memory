---
name: motor_reglas
description: Arquitectura y API del motor de reglas de tramitación ESFTT
type: project
---

Evaluador implementado en `app/services/motor_reglas.py` (PR #154).
Documento de diseño: `docs/fuentesIA/MOTOR_REGLAS_arquitectura.md`.

**Vocabulario de eventos:** CREAR / INICIAR / FINALIZAR / BORRAR

**API pública:**
```python
from app.services.motor_reglas import evaluar
res = evaluar('INICIAR', 'FASE', entidad_id=45)
res = evaluar('CREAR',   'FASE', tipo_id=8, padre_id=23)
# res.permitido, res.nivel ('BLOQUEAR'|'ADVERTIR'|''), res.mensaje, res.norma
```

**Checks hardcoded (invariantes de negocio):**
- BORRAR fase/tramite/tarea con `fecha_inicio` → BLOQUEAR
- BORRAR solicitud con fases iniciadas → BLOQUEAR
- FINALIZAR solicitud/fase/tramite con hijos abiertos → BLOQUEAR
- FINALIZAR tarea sin doc requerido → BLOQUEAR

**Why:** Tablas vacías → PERMITIDO (correcto para Fase 2; reglas las poblará el técnico).

**`figura_ambiental`** → `proyecto.ia_id` FK → `tipos_ia`. Acceso: `proyecto.ia.siglas`.
**`es_modificacion`** → campo Boolean en `proyectos` (migración aplicada).
