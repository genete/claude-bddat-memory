---
name: project-organizacion-documental-pendiente
description: #572 (pertenencia documental al EXPEDIENTE, ADR-027) no se implementa aislado — forma parte de un concepto más amplio de organización de documentos pendiente de estudiar
metadata:
  node_type: memory
  type: project
  originSessionId: 994c1951-3488-4b80-82ec-a5d55264839a
---

#572 ("Pertenencia documental al EXPEDIENTE: vínculo a tarea + flag integra_expediente",
ADR-027) no es candidato a "próximo" issue aislado aunque parezca autocontenido.

**Why:** Carlos (julio 2026) — no es que sea más grande de lo que parece, es que forma parte
de un concepto más amplio ("organización de los documentos") que aún no se ha estudiado con
vista global. Si al final hace falta migración de datos existentes, es probable que haya que
migrar más cosas de las que #572 por sí solo contempla. Implementarlo suelto arriesga forzar
una segunda migración cuando aparezca el resto del concepto.

**How to apply:** No proponer #572 como "próximo" sin que antes exista esa sesión de estudio
de alcance amplio sobre organización documental. Si aparece en una lista de candidatos,
señalar explícitamente esta reserva en vez de tratarlo como una feature aislada más.
