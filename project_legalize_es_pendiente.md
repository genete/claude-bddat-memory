---
name: Integración legalize-es — COMPLETADO
description: Resumen del trabajo realizado en sesión 2026-04-05 para integrar legalize-es y separar skills boe/boja
type: project
---

**COMPLETADO** en sesión 2026-04-05.

## Qué se hizo

1. **Clonado** `https://github.com/legalize-dev/legalize-es` en `D:\legalize-es`
2. **Creado skill `/legalize`** — búsqueda local en legalize-es (Grep/Glob/Read, sin red)
3. **Modificado skill `/boe`** — añade FLUJO 0 con `Skill(legalize)`; eliminada sección BOJA
4. **Creado skill `/boja`** — FLUJO 0 via `/legalize`; FLUJO 1 sedeboja Playwright si NOT_FOUND
5. **Permisos** `Skill(boja)` y `Skill(legalize)` añadidos en `settings.local.json`

## Estructura de legalize-es

```
D:\legalize-es\
  es/           ← BOE estatal (8 646 normas, BOE-A-*.md)
  es-an/        ← Andalucía: BOE-A-*.md (239, históricas) + BOJA-b-*.md (58, desde 2012)
  es-ct/  es-md/  ... (17 CCAA)
```

El frontmatter `title` contiene el nombre completo de la norma — se busca por Grep.

## Cuándo usar cada skill

| Skill | Cuándo |
|---|---|
| `/legalize` | Consultar disponibilidad local / primera parada de los otros |
| `/boe` | Legislación estatal BOE |
| `/boja` | Legislación andaluza BOJA/sedeboja |

**Why:** legalize-es tiene BOJA consolidado desde 2012; las normas pre-2012 siguen necesitando sedeboja.

**How to apply:** Para normas BOJA siempre `/boja`; nunca más BOJA dentro de `/boe`.
