---
name: Presentación BDDAT — Reveal.js
description: Sistema visual JA, estructura de slides y decisiones de la presentación Reveal.js para reunión interna
type: project
originSessionId: 0e35c0c7-7ca1-4659-b052-a4d7fd4f4ebd
---
## Estado
En desarrollo. Rama: `feature/292-reveal-presentacion`.
Directorio: `presentacion/` en raíz del repo. Índice en `presentacion/slides/INDICE.md`.

**Why:** Presentación interna para jefatura y compañeros. Objetivos: dar a conocer el proyecto, recoger feedback de usuarios finales, gestionar el cambio. Se publica en GitHub Pages (issue #292).

**How to apply:** Al retomar este trabajo, leer `presentacion/slides/INDICE.md` como punto de partida.

---

## Sistema visual — Plantilla JA (extraído del PDF oficial)

### Colores
- Verde institucional: `#3d8b2f` aprox (subtítulos, headers tabla, cifras, barra top)
- Verde oscuro (chevron decorativo portada): `#5a7a3a` aprox
- Gris oscuro (títulos): `#333333`
- Gris claro (body text): `#999999`, cursiva/light
- Blanco puro: `#ffffff`

### Tipografía
- Títulos: sans-serif extrabold/black (misma del CDN JA — `fonts.css`)
- Subtítulos: bold verde
- Body: light italic gris
- Notas pie: italic gris muy pequeño

### Elementos fijos en slides de contenido
- Barra verde muy fina (~6-8px) en el top (full-width)
- Numeración de sección (ej. `2.1`) + línea gris separadora, arriba-izquierda
- Título negro bold, hasta 2 líneas
- Subtítulo verde bold (opcional)
- Body gris light italic
- Pie izquierdo: "Junta de Andalucía" italic gris pequeño
- Pie derecho: "< Volver al índice | N" con enlace y número de página
- Margen derecho: "Nombre de la sección" en texto vertical gris, rotado 90°

### Portada / Portadillas
- Fondo blanco
- Chevron decorativo: dos polígonos superpuestos (verde oscuro + verde claro) en la esquina derecha, forman una "v" apuntando izquierda. Reproducible con CSS `clip-path` o SVG inline.
- Logo JA (SVG): esquina inferior derecha
- Sin barra top, sin pie de navegación
- Texto "Junta de Andalucía" italic gris en pie izquierdo (solo en portada principal)

### Layouts disponibles (del manual)
- Portada / Portadilla interior (con/sin foto de fondo)
- Contenido 1 col con foto derecha (50/50)
- Contenido 2 col texto puro
- Contenido 3 col texto puro
- Tabla estilo 1: header verde sólido, celdas con nivel 1 bold + nivel 2 italic
- Tabla estilo 2: headers gris, ítems en verde (para datos numéricos)
- Línea de tiempo: bloques numerados en cuadro verde, 3 columnas con bullets
- Cifras: icono gris + número verde grande + descripción italic

---

## Decisiones de la presentación

### Formato y flujo
- Slides en MD primero → consensuar contenido → traducir a HTML/Reveal.js final
- GitHub Pages: GitHub Actions despliega `presentacion/` a `gh-pages`
- Demo en vivo: conexión remota al PC de trabajo, HTML estático + app Flask corriendo
- Sin tiempo fijo: presentación abierta a debate; mínimo ~30 min si no hay interrupciones

### Diagramas
- NO Mermaid (rígido, feo para audiencia no técnica)
- Miro con iframe: vista específica encuadrada, el presentador controla el encuadre
- Miro requiere conexión a internet (igual que GitHub Pages — no es problema)

### Speaker notes
- Visibles solo para el presentador con tecla `S` en Reveal.js
- Se definen en los MDs de cada slide y se mapean a `<aside class="notes">` en el HTML

### Contenido
- Tono no técnico en el body principal
- Términos técnicos permitidos cuando son relevantes (JSON, Context Assembler, etc.) — la audiencia conoce XML
- Slide técnica de arquitectura en apéndice (`data-visibility="uncounted"`)
- Bloque de características (S04-S08) es el núcleo de venta: lo implementado ya

### Logo JA
- Se añade al repo en `presentacion/assets/` cuando se retome el trabajo
- Preferir SVG incrustado para funcionamiento offline

---

## Estructura de slides (ver INDICE.md para detalle completo)
S00 Portada → S01 Problema → S02 Qué es BDDAT → S03 Roles →
S04-S08 Características (interfaz, ESFTT, documental, escritos, listado) →
S09 Demo en vivo (portadilla) → S10 Roadmap → S11 Feedback →
SA1-SA2 Apéndice técnico
