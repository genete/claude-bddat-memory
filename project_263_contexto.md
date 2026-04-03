---
name: fix/263 contexto de diseño
description: Decisiones de diseño para el fix del issue #263 (bugs seguimiento) — commits listos, PR pendiente
type: project
---

## Estado
Rama `fix/263-seguimiento-ui` con 2 commits listos. PR pendiente de crear contra `develop`.

## Pendiente antes/después del PR
- **Saltos visuales al ocultar columnas** (responsive): cuando PROYECTO o TITULAR desaparecen hay un salto brusco. Queda para discutir en próxima sesión. No bloquea el PR.

## Implementado

**Bug #5 (breadcrumb):** `{% block breadcrumb %}` → `Inicio › Expedientes › Seguimiento`.

**Bug #4 (placeholder):** "Buscar por Nº AT o titular..."

**Bug #1 (badge ×2):** `.num-at-pill` como base común (todas las filas misma altura); `.num-at-multiples` añade fondo amarillo. Sin badge.

**Bug #2 (RESOLUCIÓN invisible):** ocultación progresiva por media queries. Breakpoints calibrados al espacio real (cols fijas ≈ 848px): PROYECTO ≤1100px → TITULAR ≤1000px → CONSULTAS+MA ≤991px → IP ≤767px.

**Bug #3 (espacio desperdiciado):** `table-layout:fixed`. Variables CSS `--seg-pista-w: 5.75rem` y `--seg-fixed-tot: 53rem`. Pistas con `display:block` (pill rellena celda). TITULAR/PROYECTO al 40/60% vía JS (`ajustarFlexiblesSeguimiento` + ResizeObserver). Mínimo 100px cada flexible.

**Búsqueda textual (bug extra):** `SeguimientoScroll.loadMore()` incluye `search`; API filtra por Nº AT o nombre titular.

## Decisiones de diseño clave
- `calc()` con rem no funciona en `table-layout:fixed` en Chrome → JS para el 40/60.
- `git add -p` interactivo no funciona en Bash no-TTY → un solo commit por sesión.
