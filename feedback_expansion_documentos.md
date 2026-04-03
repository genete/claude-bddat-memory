---
name: Expansión en documentos de análisis requiere supervisión
description: Al escribir en documentos de diseño, Claude expande el razonamiento del usuario con inferencias propias que a veces no están alineadas con lo que se intentó expresar
type: feedback
---

Al trasladar decisiones o razonamiento del usuario a documentos de análisis (DISEÑO_*, NORMATIVA_*, etc.), Claude tiende a expandir e interpretar, introduciendo deducciones propias que no siempre coinciden con lo que el usuario expresó. Esto genera trabajo de revisión en cada iteración.

**Why:** El usuario lo ha observado de forma recurrente a lo largo de múltiples sesiones con documentos de diseño (ver git log de DISEÑO_FECHAS_PLAZOS.md). El coste es inevitable con IA autónoma, pero debe minimizarse.

**How to apply:** Al escribir en documentos de diseño:
- Transcribir lo que el usuario dijo, no lo que se infiere de ello.
- Si hay que expandir o inferir algo para completar una sección, marcarlo explícitamente como "interpretación" o "propuesta derivada" y señalarlo al usuario antes de hacer el commit.
- Ante la duda, menos es más: escribir menos y más fiel al original antes que más y posiblemente incorrecto.
