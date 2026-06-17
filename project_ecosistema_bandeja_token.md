---
name: project_ecosistema_bandeja-token
description: Patrón de correlation token BDDAT:<n> para recuperar número de comunicación de bandeja; ecosistema de repos exploratorios relacionados
metadata: 
  node_type: memory
  type: project
  originSessionId: 9845625f-497b-4d35-982d-75ac1a9175a9
---

## Patrón de correlation token para comunicaciones de bandeja

**Hecho:** La bandeja de la Junta no muestra el número de comunicación asignado tras crear una nueva. Para recuperarlo desde BDDAT se propone inyectar un token único en el campo descripción al enviar.

**Formato acordado:** `BDDAT:<8 chars alfanuméricos opacos>` — e.g. `BDDAT:a3f9k2m8`

**Flujo:**
1. BDDAT genera el token al construir la tarea ELABORAR
2. Lo inyecta en la descripción al enviar la comunicación (via bandeja-downloader o similar)
3. Tras el envío, busca en bandeja filtrando por ese token
4. Recupera el número oficial de comunicación → lo asocia a la tarea ELABORAR en BD

**Alternativa:** usar el token como identificador definitivo sin necesitar el número de bandeja (más simple, pero pierde trazabilidad oficial).

**Why:** La bandeja es frágil para obtener el número de comunicación recién creado; no hay pantalla de confirmación con ese número.

**How to apply:** Cuando se diseñe la entidad "comunicación enviada a bandeja" dentro de tarea-ELABORAR, incluir campo `correlation_token` y campo `bandeja_num_comunicacion` (nullable hasta recuperación).

## Portafirmas — opaco para scraping

Port@firmas (v3.7.12.1) usa AutoFirma JS (MiniApplet) para autenticación → diálogo nativo OS → inaccesible a Playwright. El login usuario/clave existe en DOM pero está bloqueado por `debugMode="N"`. **Scraping inviable.** El estado de firma solo se puede inferir indirectamente desde bandeja.

**Why:** Auditado DOM completo en junio 2026 — 5 métodos de login en DOM, solo 1 visible.

## Ecosistema de repos exploratorios relacionados

Repos creados en conversaciones separadas que forman el ecosistema de integración externa:
- **bandeja-downloader** — lectura de comunicaciones de bandeja
- **ptwanda-tramitador** — extracción de ficheros de PTWANDA (solicitudes nuevas); alimentan el pool de documentos de BDDAT; scraping automático, pero incorporación al pool aún manual
- **notifica-poc** — envío de notificaciones

El token `BDDAT:<n>` es el hilo que conectaría los tres con BDDAT.
La entidad "comunicación enviada a bandeja" aún no existe en BDDAT (pendiente de diseño).
