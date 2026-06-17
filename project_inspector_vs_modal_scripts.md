---
name: project_inspector_vs_modal_scripts
description: Inspector overlay (capa 2) NO re-ejecuta los <script> de sus fragmentos; el modal grande (capa 3) SÍ — decide dónde colocar el JS pesado en cada migración ADR-023
metadata: 
  node_type: memory
  type: project
  originSessionId: ac63bd3f-5b51-4084-b402-498ba0d81b3e
---

En el patrón inspector ADR-023, las dos capas cargan fragmentos HTML de forma distinta:

- **Inspector overlay (capa 2, `inspector-overlay.js`)**: inyecta con `body.innerHTML = html` → los `<script>` del fragmento **NO se ejecutan**. El comportamiento debe delegarse a JS global (event-delegation a nivel `document`), como hacen `data-inspector-edit/form/cancel` y, en plantillas (#545), `plantillas-inspector.js` (copiar token, abrir explorador).
- **Modal grande (capa 3, `modal-large.js`)**: `_inject()` reconstruye los `<script>` → **sí se re-ejecutan**. Un fragmento con JS inline funciona tal cual.

**Por qué importa:** al migrar un listado, el JS pesado (cascadas, exploradores de ficheros, paneles con botones) se coloca en capa 3 (reuso directo del script inline) o se reescribe como delegación global si va al inspector. En #545 el panel de tokens y el explorador FS fueron a capa 3 por esto; la cascada ESFTT se evitó usando selects planos. Aplicar el mismo criterio en migraciones siguientes (seguimiento, proyectos).

**Gotcha del modal grande:** al cerrarse llama `onSaved` (por defecto `AppInspector.refresh()`). Si se abre desde una edición del inspector en curso (p. ej. el explorador de ficheros), pasar `onSaved: ()=>{}` para no refrescar y perder la edición. El atributo declarativo `data-modal-large-url` usa el default, así que ese caso necesita un handler propio.

Relacionado: [[project_react_workbench_mutaciones]], [[project_backend_solido_revamping]].
