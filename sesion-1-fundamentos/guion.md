# Guion sesión 1: Fundamentos y primer repositorio

**Duración:** 2 horas (10:00 a 12:00) | **Cupo:** 15 | **Instructores:** 2 (uno dirige, otro apoya)

**Objetivo:** al terminar, cada participante crea un repositorio local, hace commits con sentido, lo sube a GitHub y sabe corregir un error común (subir secretos).

## Cronograma

| Bloque                                       | Tipo             | Responsable       |
|----------------------------------------------|------------------|-------------------|
| Bienvenida y preguntas y respuestas          | Conversación     | A                 |
| ¿Por qué control de versiones? Git vs GitHub | Exposición corta | A                 |
| Login en GitHub y configuración de Git       | Práctica guiada  | A dirige, B apoya |
| Demo: init, status, add, commit              | Demostración     | A                 |
| Ejercicio 1: repo local con 5 commits        | Práctica         | B circula         |
| Descanso                                     |                  |                   |
| .gitignore y error del .env                  | Demo + práctica  | B                 |
| Ejercicio 2: GitHub, remote, push, pull      | Práctica         | A dirige, B apoya |
| Reto final: README y push                    | Reto             | ambos             |
| Cierre y adelanto de sesión 2                | Cierre           | A                 |

## Bienvenida y preguntas y respuestas
- Presentación de los instructores y del objetivo del taller.
- Dinámica de mano alzada: quién ha usado Git, quién ha perdido trabajo por no tener versiones.
- Ronda abierta de preguntas y respuestas para dar a conocer Git. Partimos de que nadie sabe nada: no hay cuestionario inicial, es tiempo que se aprovecha mejor conversando.
- Preguntas semilla por si el grupo no arranca: ¿cómo guardan hoy las versiones de un trabajo?, ¿qué pasa si dos personas editan el mismo archivo?, ¿para qué sirve GitHub?
- Anotar las dudas en el pizarrón y marcarlas conforme se resuelvan a lo largo de la sesión.
- Compartir el repo del taller y pedir que abran el cheatsheet.

## Por qué control de versiones
- Demo visual: carpeta con `tesis_final.docx`, `tesis_final_v2.docx`, `tesis_final_ahora_si.docx`.
- Git = herramienta local de versiones. GitHub = plataforma remota de colaboración.
- Conceptos: repositorio, commit, historial, working directory, staging area.
- Apoyo visual opcional: https://learngitbranching.js.org/

## Login en GitHub y configuración de Git

En las PCs del laboratorio **Git y Git Bash ya están instalados**: no se instala nada. Lo que hacemos aquí es
poner la identidad de cada quien y **dejar el login de GitHub resuelto desde el inicio**, para que el `push` del
ejercicio 2 no se atore con la ventana de autenticación.

Todos los comandos se escriben en **Git Bash** (menú Inicio, "Git Bash"; o clic derecho en una carpeta y
"Open Git Bash here").

### 1. Identidad de cada participante, y que la verifiquen
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@ejemplo.com"
git config --global init.defaultBranch main
git config --global --list
```
- El correo debe ser el mismo de la cuenta de GitHub, si no los commits no se asocian a su perfil.
- Quien no quiera exponer su correo puede usar el `@users.noreply.github.com` que GitHub le asigna.
- `git config --global --list` es la verificación: deben ver ahí su nombre y su correo antes de seguir.

### 2. Login con su cuenta de GitHub (aquí sale el banner)
Un solo comando, es el que nos interesa:
```bash
git credential-manager github login
```
Qué va a pasar, paso a paso:
1. Se abre la ventana de Git Credential Manager con la opción de iniciar sesión con el navegador.
2. Se abre el navegador en GitHub: usuario y contraseña.
3. Si tienen 2FA, piden el código del celular o de la app.
4. El navegador muestra "Authentication Successful" y se puede cerrar la pestaña.
5. La terminal regresa sin error: eso es el criterio de éxito.

**Si el navegador no cooperó:**
```bash
git credential-manager github login --device
```
Muestra un código para escribir en https://github.com/login/device desde el celular.

**Respaldo (token personal, hoja de pasos impresa):**
```bash
git credential-manager github login --pat
```
Se pega el token cuando lo pida. No se escribe el token en ningún archivo del repo.

### Notas para los instructores
- No damos por hecho que todos tengan cuenta de GitHub. Quien no la tenga la crea en https://github.com/signup
  y vuelve a correr el comando. El bloque completo no debe pasar de 5 minutos.
- Instructor A dirige la demo en pantalla; instructor B circula y desatora logins, que es donde se pierde más tiempo.
- Si el login falla raro, revisar que `git config --global credential.helper` diga `manager`; si sale vacío,
  correr `git credential-manager configure`.
- PC compartida: si `git credential-manager github list` muestra la cuenta de otro alumno, cerrarla con
  `git credential-manager github logout NOMBRE-DE-LA-CUENTA` antes de que el alumno entre a la suya.
- Riesgo de tiempo: 15 personas autenticándose a la vez contra la red del laboratorio. Si la red falla, se pasa
  directo al ejercicio 1 (todo local) y el login se retoma antes del ejercicio 2.
- Problema común: la ventana del navegador se abre detrás de la terminal y parece que nada pasó.
- Al cierre de la sesión hay que deshacer este login (ver "Cierre").

## Demo en vivo
```bash
mkdir mi-primer-repo && cd mi-primer-repo
git init
echo "# Mi primer repo" > README.md
git status
git add README.md
git status
git commit -m "Agrega README inicial"
git log --oneline
```
- Preguntar antes de cada comando: "¿qué creen que mostrará?".
- Explicar los tres estados: modificado, preparado (staged), confirmado.

## Ejercicio 1: repo local
Cada participante, en su repo:
1. Crear `notas.md` con 3 líneas y hacer commit.
2. Editar una línea, ver `git diff`, y hacer commit.
3. Crear `script.py` con un print y hacer commit.
4. Hacer al menos 5 commits con mensajes claros.
5. Revisar con `git log --oneline` y `git show HEAD`.

**Criterio:** 5 commits, mensajes descriptivos (no "cambios" ni "asdf").
**Error a replicar:** `git commit` sin `git add` (mensaje "nothing added to commit"). Explicar por qué.

## Descanso

## .gitignore y el error del .env
- Crear `.env` con `API_KEY=12345` y mostrar que `git status` lo detecta.
- Demostrar el error: hacer `git add .` y commit con el `.env`.
- Corrección básica:
```bash
echo ".env" >> .gitignore
git rm --cached .env
git add .gitignore
git commit -m "Deja de rastrear .env"
```
- Mensaje clave: si un secreto ya se subió a un remoto, **hay que revocar la clave**; borrar el archivo no limpia el historial. La limpieza de historial se ve en sesión 2.

## 85-105 Ejercicio 2: GitHub
1. Crear un repo vacío en GitHub (sin README).
2. Conectar y subir:
```bash
git remote add origin https://github.com/USUARIO/mi-primer-repo.git
git branch -M main
git push -u origin main
```
3. Editar un archivo desde la interfaz web, hacer commit ahí y luego `git pull` en local.
4. Alternativa: `git clone` de un repo del compañero.

**Autenticación:** ya quedó resuelta al inicio de la sesión, así que el `push` debe pasar sin pedir nada. Si vuelve a aparecer el banner, se repite `git credential-manager github login`; el instructor B resuelve los bloqueos.
**Errores a replicar:** `remote origin already exists`, `rejected (non-fast-forward)` por no hacer pull antes.

## 105-115 Reto final
- Escribir un README con título, descripción, cómo ejecutar y autor.
- Hacer commit y push.
- Compartir el enlace del repo en el canal acordado como evidencia.

## 115-120 Cierre
- Resumen: init, add, commit, push, pull, status, log, diff, .gitignore.
- **Limpieza obligatoria en las PCs compartidas**, guiada por el instructor B:
```bash
git credential-manager github logout TU-CUENTA
```
  Además: quitar `git:https://github.com` del Administrador de credenciales de Windows y cerrar sesión de GitHub en el navegador.
- Tarea opcional: terminar los primeros niveles de Learn Git Branching.
- Adelanto sesión 2: ramas, conflictos y cómo deshacer errores.

## Material de respaldo
- Plan B sin internet: ejercicios 1 y .gitignore se hacen en local.
- Cheatsheet: `cheatsheet.md`.
