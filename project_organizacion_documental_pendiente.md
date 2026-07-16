---
name: project-organizacion-documental-pendiente
description: Sesión de definición de organización documental (2026-07-16) resuelta en ADR-032 + issues #664-#667; #572 confirmado ortogonal pero diferido a propósito
metadata:
  node_type: memory
  type: project
  originSessionId: 994c1951-3488-4b80-82ec-a5d55264839a
---

La sesión de estudio de alcance amplio sobre organización documental (apuntada por Carlos
el 2026-07-14) se cerró el 2026-07-16 en
[[project_adr032_ingesta_documentos]] — dos mecanismos de entrada al pool (registrar in
situ vs. subir vía multipart), rutas relativas a `FILESYSTEM_BASE` y encaje por primera
vinculación a tarea. Issues creados: #664 (rutas relativas), #665 (pool + convención de
carpetas por código de catálogo), #666 (ingesta multipart), #667 (mover al vincular).
Todos milestone M2.

**#572** (ADR-027, migración `integra_expediente`) quedó confirmado como **ortogonal** a
esta decisión — es puro BD, no depende de nada del bloque #664-#667. Ya no aplica la
reserva original (que #572 pudiera necesitar una migración mayor sin estudiar la
organización documental completa primero).

**Why:** Carlos decidió (2026-07-16) dejar #572 en cola de todos modos, no porque siga
bloqueado sino porque prefiere diferirlo "para las pruebas" — probablemente para
verificarlo junto con el resto del bloque documental una vez #664-#667 tengan algo de
recorrido, no porque haya urgencia en desacoplarlo ya.

**How to apply:** #572 es candidato válido a "próximo" en cualquier momento a partir de
ahora — la única razón para seguir sin activarlo es la preferencia explícita de Carlos de
probarlo junto al resto, no una dependencia técnica real. Si aparece en una lista de
candidatos, ya no señalar la reserva antigua (data de antes de esta sesión); si acaso,
recordar que Carlos prefirió esperar a las pruebas del bloque #664-#667.
