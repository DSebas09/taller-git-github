# Ejercicios sesión 1

Trabaja en tu propia carpeta. Si te atoras, pregunta primero a tu compañero de al lado y luego a un instructor.

## Ejercicio 0: Verifica tu entorno (5 min)
1. `git --version` muestra una versión.
2. `git config --global user.name` y `user.email` devuelven tus datos.
3. `git credential-manager --version` muestra una versión.

## Ejercicio 1: Tu primer repositorio local (20 min)
1. Crea la carpeta `mi-primer-repo`, entra y ejecuta `git init`.
2. Crea `notas.md` con 3 líneas y revisa `git status`.
3. Haz `git add notas.md` y `git commit -m "Agrega notas iniciales"`.
4. Edita una línea, ejecuta `git diff` y anota qué ves.
5. Haz commit del cambio.
6. Crea `script.py` con `print("Hola Git")` y haz commit.
7. Repite hasta tener 5 commits con mensajes claros.
8. Ejecuta `git log --oneline` y `git show HEAD`.

**Preguntas:** ¿qué diferencia hay entre `git diff` antes y después de `git add`? ¿Qué pasa si haces commit sin `git add`?

**Error a replicar:** `git commit -m "prueba"` sin cambios preparados.

## Ejercicio 2: El error del .env (15 min)
1. Crea `.env` con `API_KEY=12345` y mira `git status`.
2. Haz `git add .` y commit (el error).
3. Corrige:
    ```bash
    echo ".env" >> .gitignore
    git rm --cached .env
    git add .gitignore
    git commit -m "Deja de rastrear .env"
    ```
4. Verifica con `git status`.

**Pregunta:** si ya lo hubieras subido a GitHub, ¿basta con borrarlo? ¿Qué más harías?

## Ejercicio 3: Subir a GitHub (20 min)
1. En GitHub crea un repo vacío `mi-primer-repo` (sin README).
2. Conecta y sube:
    ```bash
    git remote add origin https://github.com/TU_USUARIO/mi-primer-repo.git
    git branch -M main
    git push -u origin main
    ```
3. Inicia sesión en el navegador cuando GCM lo pida.
4. Edita `notas.md` desde la web y haz commit.
5. En tu terminal ejecuta `git pull` y verifica que llegó el cambio.

**Errores a replicar:**
- Repite `git remote add origin ...` y lee `remote origin already exists`.
- Haz un cambio en la web y otro en local sin pull, luego `git push` y lee `rejected (non-fast-forward)`.

## Reto final (10 min)
1. Escribe un `README.md` con título, descripción, cómo ejecutar y autor.
2. Commit y push.
3. Comparte el enlace de tu repo en el canal acordado.

## Autoevaluación
- [ ] Hice al menos 5 commits con mensajes claros
- [ ] Usé `git diff` y `git log --oneline`
- [ ] Mantuve `.env` fuera del repo con `.gitignore`
- [ ] Mi repo está en GitHub con README
- [ ] Entendí el error `rejected`

## Al terminar
Quita tus credenciales de la PC: Administrador de credenciales de Windows, Credenciales de Windows, quitar `git:https://github.com`.
