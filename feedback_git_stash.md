---
name: feedback_git_stash
description: No usar git checkout -- para descartar ficheros con cambios del usuario no relacionados con el commit
type: feedback
---

`git checkout -- <fichero>` descarta cambios del usuario sin posibilidad de recuperación. El procedimiento general de stash está en `docs/fuentesIA/REGLAS_DESARROLLO.md` §12.

**Why:** Al preparar el commit de #217, CLAUDE.md tenía cambios del usuario no relacionados. Se descartaron con `git checkout -- CLAUDE.md`, perdiendo trabajo en curso.

**How to apply:** Si un fichero tiene cambios no relacionados con el commit, preguntar al usuario qué hacer — nunca descartar unilateralmente con `git checkout --`.
