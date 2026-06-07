---
name: project_ptwanda_estudio_dom
description: "Estudio de campo del DOM de PTWANDA para ptwanda-manager; acceso, flujo de scraping y descarga mapeados"
metadata: 
  node_type: memory
  type: project
  originSessionId: 68e9f253-f970-4c7c-aed9-4bc3c7e59e2f
---

Estudio de campo del DOM de **PTWANDA** (web de tramitación de la Junta, `extranet.chap.junta-andalucia.es/veauni_ptwanda-web/`) para el futuro **ptwanda-manager**. Documento vivo en `docs/referencia/ESTUDIO_DOM_PTWANDA.md` (en curso). Sesión inicial 2026-06-05: área ENERGIA, puesto TRAMITADOR_CA, procedimiento ENERG_INST (value 7), fase SOLICITUD TELEMATICA (value 112).

Hallazgos críticos no obvios:
- **Acceso por DNI/contraseña recomendado, NO por certificado.** El certificado usa mTLS con diálogo nativo del navegador (no scrapeable por DOM) y comportamiento de auto-selección inconsistente; además la exportación de PFX está restringida por política del equipo. Basta **1 sola cuenta** (la visibilidad en PTWANDA es por área, no por funcionario). PTWANDA es multi-rol/dos pasos, como BDDAT — ver [[project_login_dos_pasos]].
- **Bloqueo permanente:** abrir un expediente (`tramitarExpediente(idInterno)`) lo **bloquea**; solo se libera **navegando a INICIO** (no por logout). Cerrar el navegador en seco lo deja bloqueado indefinidamente → el scraper debe liberar siempre en `finally`.
- **Descarga uniforme por `refdoc`:** `modulos/docsAsociadosExpediente/descargarDocumento.action?refdoc=<refdoc>` con **request directa** (`page.request`/`fetch` con la sesión) trae los bytes de PDF/ZIP/ODS por igual. El servidor manda todo `Content-Disposition: inline`; el visor de PDF es cosa del navegador, no del endpoint. Confirmado descargando un PDF y un ZIP reales.
- **Selectpicker oculto** en la búsqueda: `page.select_option` falla; fijar `select.value` + `dispatchEvent('change')` (dispara el AJAX que carga fases). Listado de resultados es DataTables paginado → mirar el contador, no las filas visibles. Tabla de documentos es DataTables **1.9** → `fnGetNodes()` para iterar todas.
- **Pendiente:** clasificación por tipo de documento — no hay id de tipo expuesto (solo `refdoc` de instancia) y el texto de tipo es ambiguo ("Documentación voluntaria no catalogada" agrupa ficheros dispares).

Relación con el diseño: alimenta DISEÑO_ECOSISTEMA_MANAGERS.md y reutiliza el patrón de ADR-021 §3 (credenciales externas cifradas en BD). La POC `D:\notifica-poc` (Notific@ PNT, usuario/contraseña) es la referencia de estilo del cliente Playwright.
