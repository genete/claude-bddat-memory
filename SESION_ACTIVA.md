# Sesión activa — Estado al 2026-02-27

## Dónde lo dejamos

Diseño completo del motor de reglas + tablas creadas en BD. Evaluador pendiente.

## Lo que se hizo en esta sesión

### Documentación
- `docs/fuentesIA/MOTOR_REGLAS_arquitectura.md` — documento de diseño completo.
  Leer entero antes de continuar. Contiene principios, firma del evaluador,
  mapa de reglas por entidad, auditoría de estados ESFTT, compatibilidad de tipos.

### Decisiones de diseño tomadas
- **Un solo evaluador**, no múltiples motores. Evento es solo un filtro `WHERE`.
- **CHECK constraint** para `entidad` en reglas_motor (no tabla separada).
  Razón: añadir entidad nueva siempre requiere código nuevo (handler), la tabla no aporta configurabilidad real.
- **SOLICITUD_TIPO eliminada** como entidad del motor. El handler de SOLICITUD
  gestiona compatibilidad de tipos directamente via TIPOS_SOLICITUDES_COMPATIBLES.
- **Whitelist** para pares compatibles de tipos en solicitud (no blacklist).
- **Estado SOLICITUD** es campo persistido en BD (`EN_TRAMITE`, `RESUELTA`, `DESISTIDA`, `ARCHIVADA`).
- **FASE usa `resultado_fase_id`** (FK a tipos_resultados_fases), no boolean `exito`.
  GuiaGeneralNueva tiene esta discrepancia — pendiente de corregir.
- **@property de estado** en FASE, TRAMITE, TAREA: pendientes de implementar.
  Solo `Solicitud.activa` existe hoy.

### Tablas creadas en BD (migraciones aplicadas)
- `public.reglas_motor` — reglas: evento + entidad + tipo_id → accion + condiciones
- `public.condiciones_regla` — condiciones booleanas 1:N con reglas_motor
- `public.tipos_solicitudes_compatibles` — whitelist pares compatibles de tipos

### Modelos creados
- `app/models/motor_reglas.py` — ReglaMotor, CondicionRegla, TipoSolicitudCompatible
- Registrados en `app/models/__init__.py`

### Entidades finales del motor
| Entidad    | tipo_id referencia       |
|------------|--------------------------|
| SOLICITUD  | tipos_solicitudes.id     |
| FASE       | tipos_fases.id           |
| TRAMITE    | tipos_tramites.id        |
| TAREA      | tipos_tareas.id          |
| EXPEDIENTE | tipos_expedientes.id     |

### Firma del evaluador (acordada, no implementada)
```python
# CREAR: entidad no existe aún
evaluar(evento='CREAR', entidad='FASE', tipo_id=8, padre_id=23, params={})

# CERRAR / BORRAR: entidad ya existe
evaluar(evento='CERRAR', entidad='FASE', entidad_id=45, params={})

# Retorna:
EvaluacionResult(permitido: bool, nivel: str, mensaje: str, norma: str)
```
`params` solo necesario en un caso: `organismo_id` para SEPARATAS.

## Próximos pasos (en orden)

1. **@property de estado** en modelos FASE, TRAMITE, TAREA (ver tabla en MOTOR_REGLAS_arquitectura.md)
2. **Evaluador** — `app/services/motor_reglas.py`:
   - Función `evaluar()` con la firma acordada
   - Implementar primero handlers de TAREA (más autónomos, sin dominio legislativo)
   - Resto como stubs con `raise NotImplementedError` o return PERMITIDO
3. **Hooks** en rutas ESFTT (POST crear/cerrar/borrar) llamando a `evaluar()`
4. **Definir pares compatibles** en TIPOS_SOLICITUDES_COMPATIBLES con el técnico
5. **Decidir dónde vive `figura_ambiental`** en el modelo (expediente, solicitud o proyecto)
6. **Corregir GuiaGeneralNueva**: `exito` (bool) → `resultado_fase_id`

## Issues abiertos relevantes
- **#151** — Infraestructura para despliegue a producción
- **#45** — SECRET_KEY antes de producción
- **#120** — Establecer base numero_at antes de producción
