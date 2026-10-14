# Ejercicios sesión 2

Hoy todo se hace en tu PC: no necesitas internet ni entrar a GitHub. Escribe los comandos en **Git Bash**. Si te atoras, pregunta primero a tu compañero de al lado y luego a un instructor.

Para crear o editar un archivo usa el editor que prefieras. Desde Git Bash puedes abrir el Bloc de notas con `notepad tareas.md`; al terminar guarda, cierra la ventana y regresa a la terminal.

Dos comandos que te van a sacar de dudas toda la sesión:
- `git status` te dice en qué rama estás y qué está pasando.
- `git log --oneline --graph --all` dibuja el historial con todas las ramas.

## Ejercicio 0: Prepara tu repositorio
Es una PC compartida, así que primero revisa que Git tenga tu nombre. Después creas el repositorio con el que vas a trabajar hoy.

1. Dile a Git quién eres y revisa que quedó bien:
    ```bash
    git config --global user.name "Tu Nombre"
    git config --global user.email "tu-correo@ejemplo.com"
    git config --global init.defaultBranch main
    git config --global --list
    ```
2. Crea el repositorio:
    ```bash
    mkdir practica-ramas
    cd practica-ramas
    git init
    ```
3. Crea el archivo `tareas.md` con exactamente estas tres líneas:
    ```
    # Tareas del equipo
    - Comprar galletas
    - Reservar el aula
    ```
4. Guarda tu primer commit:
    ```bash
    git add tareas.md
    git commit -m "Agrega lista de tareas"
    ```

**Lo lograste si:** `git log --oneline` muestra un commit y `git status` dice `nothing to commit, working tree clean`.

## Ejercicio 1: Tu primera rama
Una rama es una línea de trabajo separada: ahí puedes hacer cambios sin tocar `main`. Cuando el cambio está listo, lo unes a `main` con un merge.

1. Crea una rama y cámbiate a ella:
    ```bash
    git switch -c agrega-tarea
    git branch
    ```
    El asterisco marca la rama en la que estás.
2. Agrega al final de `tareas.md` la línea `- Imprimir las listas` y haz commit:
    ```bash
    git add tareas.md
    git commit -m "Agrega tarea de imprimir listas"
    ```
3. Regresa a `main` y mira el archivo:
    ```bash
    git switch main
    cat tareas.md
    ```
    La línea nueva no está. No se perdió: sigue guardada en la otra rama.
4. Une la rama a `main` y vuelve a mirar el archivo:
    ```bash
    git merge agrega-tarea
    cat tareas.md
    ```
    Ahora sí aparece la línea.
5. La rama ya cumplió su trabajo, bórrala:
    ```bash
    git branch -d agrega-tarea
    ```

**Lo lograste si:** `tareas.md` en `main` tiene la línea nueva y `git branch` muestra solo `main`.

**Provoca este error y arréglalo:** ejecuta `git switch prueba` y lee el error. Esa rama no existe: `git switch` solo cambia a ramas que ya existen, y para crear una nueva hace falta `-c`. No necesitas crearla.

**Pregunta:** en el paso 3, ¿por qué no veías la línea nueva si ya habías hecho commit?

## Ejercicio 2: Un conflicto
En un proyecto real `main` sigue cambiando mientras tú trabajas en tu rama. Si los dos cambian **la misma línea**, Git no puede adivinar cuál quieres y te pide decidir: eso es un conflicto.

1. Crea una rama y cambia una línea que ya existe:
    ```bash
    git switch -c cambia-aula
    ```
    En `tareas.md` cambia la línea `- Reservar el aula` por `- Reservar el aula 3` y haz commit:
    ```bash
    git add tareas.md
    git commit -m "Cambia la reserva al aula 3"
    ```
2. Regresa a `main`:
    ```bash
    git switch main
    ```
    Aquí la línea todavía dice `- Reservar el aula`. Cámbiala por `- Reservar el auditorio` y haz commit:
    ```bash
    git add tareas.md
    git commit -m "Cambia la reserva al auditorio"
    ```
3. Intenta unir:
    ```bash
    git merge cambia-aula
    ```
    Git responde `CONFLICT`. No se rompió nada: te está pidiendo que decidas.
4. Ejecuta `git status`. Te dice qué archivo tiene el conflicto (`both modified`).
5. Abre `tareas.md`. Git marcó la parte en conflicto así:
    ```
    <<<<<<< HEAD
    - Reservar el auditorio
    =======
    - Reservar el aula 3
    >>>>>>> cambia-aula
    ```
    Lo que está arriba de `=======` es la versión de `main`, donde estás parado. Lo de abajo es la versión de la rama que estás uniendo.
6. Deja la línea como quieras que quede y borra las tres líneas de marcas: `<<<<<<<`, `=======` y `>>>>>>>`. Guarda el archivo.
7. Avísale a Git que ya lo resolviste y termina el merge:
    ```bash
    git add tareas.md
    git commit -m "Resuelve conflicto de la reserva"
    ```
8. Mira el resultado con `git log --oneline --graph`: se ve dónde se separaron las dos historias y dónde se unieron.

**Lo lograste si:** `git status` dice `working tree clean` y `tareas.md` ya no tiene ninguna línea de marcas.

**Si te arrepientes a medio conflicto:** `git merge --abort` cancela el merge y deja todo como estaba antes de intentarlo.

**Pregunta:** ¿por qué en el ejercicio 1 no hubo conflicto y aquí sí?

## Ejercicio 3: Deshacer cambios
Git tiene una forma distinta de deshacer según si el cambio ya tiene commit o no:

| Qué pasó | Qué usar |
|---|---|
| Cambié un archivo y todavía no hago commit | `git restore` |
| Ya hice commit y me equivoqué | `git revert` |

1. **Descarta un cambio sin commit.** Borra varias líneas de `tareas.md` y guarda. Después:
    ```bash
    git status
    git restore tareas.md
    cat tareas.md
    ```
    El archivo regresó a como estaba en el último commit.
2. **Anula un commit.** Agrega al final de `tareas.md` la línea `- Cancelar el taller` y haz commit:
    ```bash
    git add tareas.md
    git commit -m "Agrega tarea equivocada"
    ```
    Ahora anúlalo:
    ```bash
    git revert --no-edit HEAD
    git log --oneline
    cat tareas.md
    ```
    `HEAD` es el último commit. El commit equivocado sigue en el historial, pero hay uno nuevo que deshace lo que hizo.

**Lo lograste si:** `tareas.md` no tiene la línea `- Cancelar el taller` y `git log --oneline` muestra el commit `Revert "Agrega tarea equivocada"`.

**Pregunta:** ¿por qué en el paso 2 no bastaba con `git restore`?

### Extra opcional: recuperar commits que parecían perdidos
Hazlo solo si terminaste antes.

1. Ejecuta `git log --oneline` y cuenta cuántos commits hay.
2. Borra los últimos tres commits:
    ```bash
    git reset --hard HEAD~3
    git log --oneline
    ```
    Ya no aparecen tus últimos commits. Parece que se perdieron.
3. Git anota cada movimiento que haces. Míralo:
    ```bash
    git reflog
    ```
    La primera línea dice `reset: moving to HEAD~3`: es lo que acabas de hacer. La línea de abajo es donde estabas antes. Copia el código con el que empieza esa segunda línea.
4. Regresa a ese punto, poniendo tu código en lugar de `CODIGO`:
    ```bash
    git reset --hard CODIGO
    git log --oneline
    ```
    Volvieron todos.

## Reto final
Ahora tú solo, sin pasos detallados:

1. Crea una rama `reto` y cambia en ella la línea `- Comprar galletas`.
2. Regresa a `main` y cambia esa misma línea por otra cosa.
3. Une las ramas y resuelve el conflicto.
4. Muéstrale a un instructor tu `git log --oneline --graph`.
