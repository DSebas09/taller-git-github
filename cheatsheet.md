# Cheatsheet de Git

Lo que está entre `< >` lo cambias por tu dato, sin los signos. Ejemplo: `git add <archivo>` se escribe `git add notas.md`.

## Antes de empezar
| Comando | Cuándo usarlo |
|---|---|
| `git config --global user.name "Tu Nombre"` | Al llegar a una PC compartida: pone tu nombre en tus commits. |
| `git config --global user.email "tu-correo"` | Igual que el anterior, con el correo de tu cuenta de GitHub. |
| `git config --global --list` | Para revisar con qué nombre y correo estás firmando. |

## Guardar tu trabajo
| Comando | Cuándo usarlo |
|---|---|
| `git init` | Para convertir una carpeta en repositorio. Una sola vez por proyecto. |
| `git status` | Cuando no sabes qué está pasando: en qué rama estás, qué cambió y qué falta. Úsalo seguido. |
| `git diff` | Para ver qué cambiaste antes de hacer `git add`. |
| `git add <archivo>` | Para elegir qué cambios van en el siguiente commit. |
| `git commit -m "Agrega ..."` | Para guardar una foto del proyecto. El mensaje dice qué hace el cambio. |
| `git log --oneline` | Para ver la lista de commits. |
| `git show HEAD` | Para ver qué cambió en el último commit. |

## Archivos que no deben subirse
| Comando | Cuándo usarlo |
|---|---|
| `echo "<archivo>" >> .gitignore` | Para que Git ignore un archivo, como `.env` con contraseñas o claves. |
| `git rm --cached <archivo>` | Si ya hiciste commit de ese archivo: Git deja de rastrearlo, pero sigue en tu carpeta. |

## Trabajar con GitHub
| Comando | Cuándo usarlo |
|---|---|
| `git credential-manager github login` | Al inicio, para entrar a tu cuenta de GitHub desde Git Bash. |
| `git clone <url>` | Para bajar a tu PC un repo que ya existe en GitHub. |
| `git remote add origin <url>` | Para conectar tu repo con uno vacío de GitHub. Una sola vez. |
| `git remote -v` | Para ver a qué repo de GitHub está conectado el tuyo. |
| `git push -u origin main` | La primera vez que subes tus commits. |
| `git push` | Las siguientes veces que subes tus commits. |
| `git pull` | Para traer los cambios que hay en GitHub y tú no tienes. Hazlo antes de empezar a trabajar. |
| `git credential-manager github list` | Al terminar en una PC compartida, para ver con qué cuenta entraste. |
| `git credential-manager github logout <cuenta>` | Después del anterior, para cerrar tu cuenta. |

## Ramas
| Comando | Cuándo usarlo |
|---|---|
| `git branch` | Para ver las ramas. El `*` marca en cuál estás. |
| `git switch -c <rama>` | Antes de empezar un cambio: crea una rama nueva y te cambia a ella. |
| `git switch <rama>` | Para cambiarte a una rama que ya existe. Haz commit antes de cambiarte. |
| `git merge <rama>` | Para traer el trabajo de otra rama. Párate primero en la rama que lo recibe, casi siempre `main`. |
| `git branch -d <rama>` | Para borrar una rama que ya uniste. |
| `git log --oneline --graph --all` | Para ver el dibujo del historial con todas las ramas. |

## Si hay un conflicto
1. `git status` te dice qué archivo tiene el conflicto.
2. Abre el archivo, deja la versión que quieres y borra las marcas `<<<<<<<`, `=======` y `>>>>>>>`.
3. Termina con `git add <archivo>` y `git commit -m "Resuelve conflicto de ..."`.

Si prefieres cancelar: `git merge --abort` deja todo como estaba antes del merge.

## Deshacer
| Comando | Cuándo usarlo |
|---|---|
| `git restore <archivo>` | Cambiaste un archivo, todavía no haces commit y no quieres ese cambio. Lo que descartas así no se recupera. |
| `git revert --no-edit HEAD` | Ya hiciste commit y te equivocaste. Crea un commit nuevo que lo deshace; es seguro aunque ya esté en GitHub. |
| `git reflog` | Para encontrar commits que parecían perdidos. Muestra todo lo que has hecho, con su código. |
| `git reset --hard <codigo>` | Después de `git reflog`, para regresar al punto de ese código. Cuidado: borra los cambios que no tengan commit. |

## Si la terminal parece trabada
- Hiciste `git commit` sin `-m` y se abrió el editor Vim: escribe `:q!` y Enter, y repite el commit con `-m "tu mensaje"`.
- Se abrió Vim durante un merge: escribe `:wq` y Enter para aceptar el mensaje.
- `git log` llenó la pantalla y no regresa: presiona `q`.
