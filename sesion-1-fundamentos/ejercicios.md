# Ejercicios sesión 1

Trabaja en tu propia carpeta y escribe todos los comandos en **Git Bash**. Si te atoras, pregunta primero a tu compañero de al lado y luego a un instructor.

## Ejercicio 0: Configura Git y entra a tu cuenta de GitHub
Git necesita saber quién eres para firmar tus commits, y GitHub necesita confirmar que eres tú antes de dejarte subir cambios.

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
3. Se abre una ventana: elige iniciar sesión con el navegador.
4. Pon tu usuario y contraseña de GitHub, y el código de 2FA si lo tienes.
5. El navegador muestra "Authentication Successful" y puedes cerrar la pestaña.

**Lo lograste si:** `git config --global --list` muestra tu nombre y tu correo, y la terminal regresó sin error después del login.

**Si algo falla:**
- No tienes cuenta de GitHub: créala en https://github.com/signup y vuelve a correr el comando del paso 2.
- El navegador no abre: usa `git credential-manager github login --device` y escribe el código en https://github.com/login/device.
- Parece que no pasó nada: revisa si la ventana del navegador quedó detrás de la terminal.

## Ejercicio 1: Tu primer repositorio local
Un repositorio es una carpeta donde Git guarda el historial de tus cambios. Cada commit es una foto del proyecto en un momento dado.

1. Crea la carpeta `mi-primer-repo`, entra y ejecuta `git init`.
2. Crea `notas.md` con 3 líneas y revisa `git status`.
3. Haz `git add notas.md` y `git commit -m "Agrega notas iniciales"`.
4. Edita una línea, ejecuta `git diff` y anota qué ves.
5. Haz commit del cambio.
6. Crea `script.py` con `print("Hola Git")` y haz commit.
7. Repite hasta tener 5 commits. Cada mensaje debe decir qué hace el cambio, por ejemplo "Corrige la segunda línea de notas"; no uses "cambios" ni "asdf".
8. Ejecuta `git log --oneline` y `git show HEAD`.

**Lo lograste si:** `git log --oneline` muestra al menos 5 commits y cada mensaje dice qué hace el cambio.

**Provoca este error y arréglalo:** edita `notas.md` y ejecuta `git commit -m "prueba"` sin hacer `git add`. Lee lo que responde Git. Después ejecuta `git add notas.md` y repite el commit.

**Preguntas:** ¿qué diferencia hay entre `git diff` antes y después de `git add`? ¿Qué pasa si haces commit sin `git add`?

**Si la terminal parece trabada después de un `git commit`:** olvidaste el `-m` y se abrió el editor Vim. Escribe `:q!` y Enter para salir, y repite el commit con `-m "tu mensaje"`.

## Ejercicio 2: Archivos que no deben subirse
Hay archivos que nunca deben entrar al repositorio, como los que guardan contraseñas o claves. El archivo `.gitignore` le dice a Git cuáles debe ignorar.

1. Crea `.env` con `API_KEY=12345` y mira `git status`.
2. Haz `git add .` y un commit.
3. Acabas de guardar una clave en tu historial. Corrígelo:
    ```bash
    echo ".env" >> .gitignore
    git rm --cached .env
    git add .gitignore
    git commit -m "Deja de rastrear .env"
    ```
4. Verifica con `git status`.

**Lo lograste si:** `git status` ya no menciona `.env` y el archivo sigue existiendo en tu carpeta.

**Pregunta:** si ya lo hubieras subido a GitHub, ¿basta con borrarlo? ¿Qué más harías?

## Ejercicio 3: Subir a GitHub
Hasta ahora todo vive en tu PC. Al conectar tu repositorio con GitHub puedes respaldarlo, compartirlo y traer cambios hechos en otro lado.

1. En GitHub crea un repo vacío `mi-primer-repo` (sin README).
2. Conecta y sube:
    ```bash
    git remote add origin https://github.com/TU_USUARIO/mi-primer-repo.git
    git branch -M main
    git push -u origin main
    ```
    Ya iniciaste sesión en el ejercicio 0, así que el `push` no debe pedirte nada. Si aparece una ventana para iniciar sesión, hazlo con el navegador.
3. Edita `notas.md` desde la web de GitHub y haz commit ahí.
4. En tu terminal ejecuta `git pull` y verifica que llegó el cambio.

**Lo lograste si:** tu repo en GitHub muestra tus commits y, después del `git pull`, tu `notas.md` local tiene el cambio que hiciste en la web.

**Provoca este error y arréglalo:** repite el `git remote add origin ...` del paso 2 y lee el error. El remoto ya estaba conectado, no hay que agregarlo otra vez. Compruébalo con `git remote -v`.

**Observa:** en GitHub abre el historial de commits y entra al commit donde agregaste el `.env`. ¿Sigue visible la `API_KEY`?

### Extra opcional: el `push` rechazado
Hazlo solo si terminaste antes. Este error lo veremos a fondo en una sesión más adelante.

1. Edita `notas.md` en la web de GitHub y haz commit ahí.
2. Sin hacer pull, edita `script.py` en tu terminal y haz commit. Usa **archivos distintos** en la web y en local, como dicen estos pasos.
3. Ejecuta `git push` y lee el error: GitHub tiene un commit que tú no tienes.
4. Arréglalo:
    ```bash
    git pull --no-rebase --no-edit
    git push
    ```
    `git pull` trae ese commit y lo une con el tuyo; después el `push` ya pasa.

## Reto final
1. Escribe un `README.md` con un título, una descripción de qué hay en tu repo (qué es cada archivo) y tu nombre como autor.
2. Haz commit y push.
3. Abre tu repo en GitHub y muéstraselo a un instructor.

## Al terminar
Es una PC compartida, no dejes tu cuenta abierta:
1. Mira con qué cuenta entraste y ciérrala:
    ```bash
    git credential-manager github list
    git credential-manager github logout TU-CUENTA
    ```
    En `TU-CUENTA` va el nombre que te mostró el primer comando. Si vuelves a correr `git credential-manager github list` y ya no muestra nada, quedó cerrada.
2. Cierra tu sesión de GitHub en el navegador.
