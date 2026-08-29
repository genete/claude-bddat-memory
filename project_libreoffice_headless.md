---
name: project_libreoffice_headless
description: "Cómo invocar LibreOffice headless en este equipo: usar soffice.com (no .exe), perfil aparte, y convertir a pdf/odt/docx/png"
metadata: 
  node_type: memory
  type: project
  originSessionId: 531a852e-572d-4ed1-8e56-36bd0a5c26a4
  modified: 2026-08-29T07:45:14.481Z
---

LibreOffice 7.6.7.2 instalado en `C:\Program Files\LibreOffice\program\`. Es
**requisito de instalación de BDDAT** por ADR-035 (plantillas de escritos en
ODT), así que se puede dar por presente.

**Usar `soffice.com`, no `soffice.exe`.** El `.exe` no devuelve el control a la
consola: `soffice.exe --version` se queda colgado hasta agotar el timeout. El
`.com` es el envoltorio de consola y sí retorna.

Patrón de invocación, con perfil de usuario aparte para no chocar con una
instancia abierta por el usuario:

```bash
SOFFICE="/c/Program Files/LibreOffice/program/soffice.com"
PERFIL="-env:UserInstallation=file:///D:/BDDAT/docs_prueba/temp/loprofile"
"$SOFFICE" --headless --norestore "$PERFIL" --convert-to pdf --outdir <dir> <fichero>
```

Filtros útiles: `odt`, `pdf`, `'docx:MS Word 2007 XML'`, y para inspección
visual `'png:draw_png_Export:{"PixelWidth":{"type":"long","value":"1240"}}'`
(solo la primera página).

Opciones del filtro PDF como JSON, p. ej. PDF/A-2b:
`'pdf:writer_pdf_Export:{"SelectPdfVersion":{"type":"long","value":"2"}}'`

**La conversión headless usa los valores por defecto del filtro**, que no tienen
por qué coincidir con lo que el usuario tenga guardado en el diálogo "Exportar
como PDF" de la ventana de Writer. Comprobado en la sesión de #182 que para ese
caso daban el mismo resultado, pero no es garantía general: si algo depende de
una opción de exportación, pedir a Carlos que lo haga desde la GUI.

**Cuidado con el render a PNG como prueba visual:** dibuja mal los acentos
compuestos con glifos combinantes (se ve "Coñsejería" donde el PDF pone
"Consejería"). Es defecto del render de Draw, no del PDF — para juzgar
apariencia, abrir el PDF en el navegador integrado (`preview_start` con
`file:///…`), que usa pdfium.

Ver [[project_verif_arbol_react]] para la verificación en navegador (Playwright MCP).
