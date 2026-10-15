# Ejercicios sesión 3

Hoy trabajas en equipo, en un repo que comparten en GitHub. Escribe los comandos en **Git Bash**. Si te atoras, pregunta primero a tu equipo y luego a un instructor.

Para editar el glosario desde Git Bash usa `notepad glosario.md`; al terminar guarda, cierra la ventana y regresa a la terminal. Escribe tus términos **sin acentos**, igual que el resto del archivo.

El flujo que vas a repetir hoy:

1. **Issue:** anotas en GitHub qué vas a hacer.
2. **Rama:** trabajas en tu PC sin tocar `main`.
3. **Commit y push:** guardas tu cambio y lo subes a GitHub.
4. **Pull request (PR):** pides que tu rama se una a `main`.
5. **Revisión:** un compañero lee tu cambio y lo aprueba.
6. **Merge:** tu cambio entra a `main` y todos lo traen con `git pull`.

## Ejercicio 0: Configura Git y entra a tu cuenta de GitHub
Es una PC compartida: primero revisa que Git tenga tu nombre y después entra a tu cuenta para poder subir cambios.

1. Dile a Git quién eres y revisa que quedó bien:
    ```bash
    git config --global user.name "Tu Nombre"
    git config --global user.email "tu-correo@ejemplo.com"
    git config --global init.defaultBranch main
    git config --global --list
    ```
    Usa el mismo correo de tu cuenta de GitHub.
2. Entra a tu cuenta de GitHub:
    ```bash
    git credential-manager github login
    ```
3. Se abre una ventana: elige iniciar sesión con el navegador. Pon tu usuario y contraseña de GitHub, y el código de 2FA si lo tienes.
4. El navegador muestra "Authentication Successful" y puedes cerrar la pestaña.

**Lo lograste si:** `git config --global --list` muestra tu nombre y tu correo, y la terminal regresó sin error después del login.

**Si algo falla:**
- El navegador no abre: usa `git credential-manager github login --device` y escribe el código en https://github.com/login/device.
- Parece que no pasó nada: revisa si la ventana del navegador quedó detrás de la terminal.

## Ejercicio 1: Entra al repo de tu equipo
Cada equipo tiene un repo en GitHub con un glosario de Git. Un instructor te invitó a ese repo con tu usuario: al aceptar, puedes subir cambios ahí.

1. Abre esta página, cambiando `N` por el número de tu equipo, y acepta la invitación:
    ```
    https://github.com/taller-git-itesi-2026/equipo-N/invitations
    ```
2. En la página del repo haz clic en el botón verde **Code** y copia la dirección que aparece en la pestaña **HTTPS**.
3. Clona el repo en tu PC. El `cd ~` te saca de cualquier repo de los días anteriores:
    ```bash
    cd ~
    git clone https://github.com/taller-git-itesi-2026/equipo-N.git
    cd equipo-N
    git log --oneline
    cat glosario.md
    ```
4. Pónganse de acuerdo en el equipo:
    - Qué sección del glosario le toca a cada quien: integrante 1, "Guardar cambios"; integrante 2, "Ramas"; integrante 3, "GitHub". Si son dos, usen las dos primeras.
    - Quién revisa a quién: el 1 revisa al 2, el 2 al 3 y el 3 al 1. Si son dos, se revisan entre ustedes.

**Lo lograste si:** `git status` dice `On branch main` y `Your branch is up to date with 'origin/main'`.

**Pregunta:** el repo es público y cualquiera puede clonarlo. ¿Qué impide que cualquiera suba cambios?

## Ejercicio 2: Tu primer pull request
Vas a agregar un término al glosario siguiendo el flujo completo. Ideas de términos por sección:

| Sección | Ideas |
|---|---|
| Guardar cambios | staging area, git status, git diff, git log, .gitignore |
| Ramas | merge, conflicto, HEAD, fast-forward, git revert |
| GitHub | clone, push, pull, issue, pull request |

1. **Crea un issue.** En GitHub, en el repo de tu equipo, entra a la pestaña **Issues** y haz clic en **New issue**.
    - Título: `Agregar termino merge al glosario`, con tu término en lugar de `merge`.
    - A la derecha, en **Assignees**, haz clic en **assign yourself**.
    - Haz clic en **Create**. Anota el número que te dio GitHub, por ejemplo `#1`.
2. **Crea tu rama.** En lugar de `TUNOMBRE` pon tu nombre, sin espacios ni acentos, por ejemplo `agrega-termino-ana`. Cada integrante usa un nombre distinto:
    ```bash
    git switch main
    git pull
    git switch -c agrega-termino-TUNOMBRE
    ```
3. **Agrega tu término.** Abre el glosario con `notepad glosario.md` y agrega una línea al final de **tu sección**, con el mismo formato que la que ya está:
    ```
    - **merge**: unir el trabajo de una rama con otra.
    ```
    Solo toca tu sección: así no chocas con tus compañeros. Guarda y cierra.
4. **Haz commit y súbelo.** La primera vez que subes una rama nueva le dices a Git a dónde va con `-u origin` y el nombre de la rama:
    ```bash
    git add glosario.md
    git commit -m "Agrega termino merge al glosario"
    git push -u origin agrega-termino-TUNOMBRE
    ```
5. **Abre el pull request.** En GitHub aparece un aviso con el botón **Compare & pull request**: haz clic.
    - El título ya trae tu mensaje de commit.
    - En la descripción escribe `Closes #1`, con el número de tu issue. Así el issue se cierra solo cuando se haga el merge.
    - A la derecha, en **Reviewers**, elige al compañero que te revisa.
    - Haz clic en **Create pull request**.
6. **Revisa el PR de tu compañero.** Entra a la pestaña **Pull requests** y abre el suyo.
    - En **Files changed** lee su cambio. Si quieres comentar una línea, pasa el mouse sobre ella y haz clic en el `+`.
    - Haz clic en **Review changes**, elige **Approve** y luego **Submit review**.
7. **Haz merge de tu PR** cuando te aprueben: **Merge pull request**, luego **Confirm merge** y al final **Delete branch**. Entra a la pestaña **Issues**: tu issue se cerró solo.
8. **Trae los cambios a tu PC** y borra tu rama, que ya está en `main`:
    ```bash
    git switch main
    git pull
    git branch -d agrega-termino-TUNOMBRE
    cat glosario.md
    ```

**Lo lograste si:** tu PR dice **Merged**, tu issue está cerrado y `glosario.md` en tu PC tiene tu término.

**Provoca este error:** en el paso 5, antes de que te revisen, intenta hacer merge de tu propio PR o aprobarlo tú mismo. GitHub no te deja: `main` está protegida y pide la aprobación de otra persona.

**Si `git branch -d` dice `not fully merged`:** hiciste el `git pull` antes del merge. Revisa que tu PR diga **Merged** y repite `git pull` y `git branch -d`.

**Pregunta:** ¿qué ganas con que otra persona lea tu cambio antes de que entre a `main`?

## Ejercicio 3: El push rechazado y `git pull --rebase`
GitHub rechaza tu `push` cuando tiene commits que tú no tienes: no quiere que pierdas el trabajo de otra persona. En equipo pasa seguido: alguien más subió algo a la rama mientras tú trabajabas.

La solución es `git pull --rebase`: trae lo de GitHub y pone tus commits **encima**, uno tras otro. El historial queda en línea recta y después el `push` ya pasa.

1. **Prepara una rama nueva** con un segundo término en tu sección:
    ```bash
    git switch main
    git pull
    git switch -c mas-terminos-TUNOMBRE
    ```
    Agrega el término al final de tu sección y súbelo:
    ```bash
    git add glosario.md
    git commit -m "Agrega termino git log al glosario"
    git push -u origin mas-terminos-TUNOMBRE
    ```
2. **Simula que un compañero subió algo a tu rama.** En GitHub, en el repo de tu equipo:
    - Arriba de la lista de archivos está el botón de ramas, que dice `main`. Cámbialo a `mas-terminos-TUNOMBRE`.
    - Abre `glosario.md` y haz clic en el lápiz para editarlo.
    - Edita solo **la primera línea de tu sección** (el término que ya venía). Agrégale un ejemplo al final, por ejemplo `Ejemplo: git commit -m "Agrega notas"`.
    - Haz clic en **Commit changes** y confirma.
3. **En tu PC, sin hacer pull**, agrega un tercer término al final de tu sección e intenta subirlo:
    ```bash
    git add glosario.md
    git commit -m "Agrega termino git diff al glosario"
    git push
    ```
    Git responde `! [rejected] ... (fetch first)`: GitHub tiene el commit que hiciste en la web y tú no.
4. **Trae lo de GitHub y pon tu commit encima:**
    ```bash
    git pull --rebase
    git push
    ```
    Git responde `Successfully rebased` y ahora el `push` sí pasa.
5. **Mira el resultado:**
    ```bash
    git log --oneline --graph -5
    cat glosario.md
    ```
    Tu commit quedó arriba del que hiciste en la web, en línea recta. El archivo tiene los dos cambios.

**Lo lograste si:** el `push` del paso 4 pasó, `git log --oneline --graph -5` no muestra un commit de merge y `glosario.md` tiene el ejemplo y tu tercer término.

**Si el `git pull --rebase` dice `CONFLICT`:** editaste la misma línea en la web y en tu PC. Cancela el rebase y haz el pull con un merge, para resolver el conflicto como en la sesión 2:
```bash
git rebase --abort
git pull --no-rebase --no-edit
```
`--no-rebase` le dice a Git que una las dos historias con un merge, y `--no-edit` acepta el mensaje del merge sin abrir el editor. Resuelve el conflicto en `glosario.md` como en la sesión 2 y termina con `git add glosario.md`, `git commit -m "Resuelve conflicto del glosario"` y `git push`.

**Si algo salió raro:** no borres la carpeta. `git reflog` muestra todo lo que hiciste; pide ayuda a un instructor y desde ahí se recupera.

**Pregunta:** en el paso 3 tu commit no tenía nada malo. ¿Por qué GitHub lo rechazó?

### Extra opcional: un fork para contribuir a otro equipo
Hazlo solo si tu equipo ya terminó el reto. Un fork es una copia de un repo ajeno en tu cuenta: sirve para proponer cambios a un proyecto donde no te invitaron.

1. Abre el repo de otro equipo en GitHub y haz clic en **Fork**, luego en **Create fork**. Ahora tienes una copia en tu cuenta.
2. En tu fork, abre `glosario.md`, haz clic en el lápiz y agrega un término en la sección "GitHub". Haz clic en **Commit changes**.
3. Haz clic en **Contribute** y luego en **Open pull request**. Escribe un título y haz clic en **Create pull request**.
4. Avísale al otro equipo: ellos revisan tu PR y deciden si hacen merge.

## Reto final
Ahora tú solo, sin pasos detallados: lleva tu rama `mas-terminos-TUNOMBRE` hasta `main`.

1. Crea un issue para tus términos nuevos.
2. Abre el PR de tu rama, enlázalo al issue con `Closes #` y pide revisión.
3. Revisa el PR de tu compañero.
4. Cuando te aprueben, haz merge, trae los cambios con `git pull` y borra tu rama.
5. Muéstrale a un instructor el glosario de tu equipo en GitHub, con los términos de todos.

## Al terminar
Es una PC compartida, no dejes tu cuenta abierta:
1. Mira con qué cuenta entraste y ciérrala:
    ```bash
    git credential-manager github list
    git credential-manager github logout TU-CUENTA
    ```
    En `TU-CUENTA` va el nombre que te mostró el primer comando. Si vuelves a correr `git credential-manager github list` y ya no muestra nada, quedó cerrada.
2. Cierra tu sesión de GitHub en el navegador.
