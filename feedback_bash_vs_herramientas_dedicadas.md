---
name: feedback_bash_vs_herramientas_dedicadas
description: No usar Bash para ls/find/grep/cat cuando Glob/Grep/Read cubren el caso — ni siquiera para consultas simples de una vez
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 09dc9729-d1cc-428d-a697-4451781cbd79
  modified: 2026-08-03T08:21:43.034Z
---

Usé `Bash` con `ls docs/decisiones/ | sort ... | tail -5` para ver el ADR más reciente,
teniendo `Glob` disponible para exactamente ese caso (`docs/decisiones/ADR-*.md`). Carlos
rechazó el permiso explícitamente en vez de aprobarlo.

**Por qué:** no es solo una preferencia de estilo — si Carlos aprueba una llamada así, el
propio mecanismo de permisos puede consolidar el patrón en `settings` (allowlist), lo que
normalizaría exactamente el hábito que no quiere. Rechazar en vez de aprobar es la forma de
cortarlo de raíz. Ya está explícito en las instrucciones de sistema ("File search: Use Glob NOT
find or ls") — el fallo no es de conocimiento sino de aplicarlo de forma consistente, incluida
cualquier consulta que parezca trivial o de una sola vez.

**Cómo aplicar:** antes de cualquier `Bash` que sea principalmente listar/buscar/leer
ficheros, comprobar primero si Glob/Grep/Read lo resuelve — no reservarlo solo para tareas
"grandes". Bash queda para lo que de verdad requiere shell (git, gh, scripts, entornos
virtuales, procesos). Distinto de [[feedback_antibloqueos_bash]] (anti-patrones de sintaxis que
bloquea el hook `reglas_bash_guard.py`) — esto es elección de herramienta, no sintaxis; no lo
cubre ningún hook.
