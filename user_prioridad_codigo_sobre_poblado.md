---
name: user-prioridad-codigo-sobre-poblado
description: Carlos prefiere dedicar su tiempo a issues que generan código; evita los de poblado puro de catálogos/tablas estructurales
metadata: 
  node_type: memory
  type: user
  originSessionId: 7460fb94-f47b-4c29-b4fa-d908d81ee8e2
---

Carlos prioriza issues que producen código (modelos, CRUD, servicios, UI) sobre
issues de poblado puro de catálogos/tablas estructurales (seeds/migraciones de
contenido sin lógica nueva) — "mientras pueda", los evita, prefiere dedicar el
tiempo a generar código.

Ejemplo (2026-07-06): con #593 y #591 cerrados, quedaban desbloqueados #441
(poblado `catalogo_requerimientos`) y #594 (`items_tecnicos` + CRUD Supervisor,
genera código real) — eligió #594 explícitamente por este criterio.

Al sugerir próximos issues o priorizar backlog, ordenar los de poblado puro
después de los que generan código, salvo que Carlos indique lo contrario o el
poblado sea un prerrequisito bloqueante de otro trabajo.
