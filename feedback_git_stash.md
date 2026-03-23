---
name: feedback_git_stash
description: Usar git stash en lugar de git checkout -- cuando hay cambios no relacionados en ficheros del usuario
type: feedback
---

Nunca usar `git checkout -- <fichero>` para descartar cambios no relacionados antes de un commit. El usuario puede tener modificaciones en curso en ese fichero.

**Why:** Al preparar el commit de #217, CLAUDE.md tenía cambios del usuario no relacionados. Los descarté con `git checkout -- CLAUDE.md` en lugar de hacer stash, perdiendo el trabajo del usuario.

**How to apply:** Cuando hay ficheros modificados que no forman parte del commit, hacer `git stash` antes de commitear y `git stash pop` después. O preguntar al usuario qué hacer con esos cambios.
