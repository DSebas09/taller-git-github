# Guion sesión 1: Fundamentos y primer repositorio

**Duración:** 2 horas (10:00 a 12:00) | **Cupo:** 15 | **Instructores:** 2 (uno dirige, otro apoya)

**Objetivo:** al terminar, cada participante crea un repositorio local, hace commits con sentido, lo sube a GitHub y sabe corregir un error común (subir secretos).

## Cronograma

| Bloque                                       | Tipo             | Responsable       |
|----------------------------------------------|------------------|-------------------|
| Bienvenida y cuestionario inicial            | Evaluación       | A                 |
| ¿Por qué control de versiones? Git vs GitHub | Exposición corta | A                 |
| Instalación y configuración                  | Práctica guiada  | A dirige, B apoya |
| Demo: init, status, add, commit              | Demostración     | A                 |
| Ejercicio 1: repo local con 5 commits        | Práctica         | B circula         |
| Descanso                                     |                  |                   |
| .gitignore y error del .env                  | Demo + práctica  | B                 |
| Ejercicio 2: GitHub, remote, push, pull      | Práctica         | A dirige, B apoya |
| Reto final: README y push                    | Reto             | ambos             |
| Cierre y adelanto de sesión 2                | Cierre           | A                 |

## Bienvenida y cuestionario inicial
- Presentación, dinámica de mano alzada: quién ha usado Git, quién ha perdido trabajo por no tener versiones.
- Aplicar `evaluacion/cuestionario-inicial.md` (10 preguntas, sin presión, no cuenta como calificación).
- Compartir el repo del taller y pedir que abran el cheatsheet.

## Por qué control de versiones
- Demo visual: carpeta con `tesis_final.docx`, `tesis_final_v2.docx`, `tesis_final_ahora_si.docx`.
- Git = herramienta local de versiones. GitHub = plataforma remota de colaboración.
- Conceptos: repositorio, commit, historial, working directory, staging area.
- Apoyo visual opcional: https://learngitbranching.js.org/

## Instalación y configuración
```bash
git --version
git config --global user.name "Tu Nombre"
git config --global user.email "correo@ejemplo.com"
git config --global init.defaultBranch main
git config --list
```
- Instructor B revisa que todos tengan salida correcta de `git --version`.
- Problemas comunes: Git no está en el PATH, correo distinto al de GitHub.

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

**Autenticación:** usar Git Credential Manager o token personal; el instructor B resuelve los bloqueos.
**Errores a replicar:** `remote origin already exists`, `rejected (non-fast-forward)` por no hacer pull antes.

## 105-115 Reto final
- Escribir un README con título, descripción, cómo ejecutar y autor.
- Hacer commit y push.
- Compartir el enlace del repo en el canal acordado como evidencia.

## 115-120 Cierre
- Resumen: init, add, commit, push, pull, status, log, diff, .gitignore.
- Tarea opcional: terminar los primeros niveles de Learn Git Branching.
- Adelanto sesión 2: ramas, conflictos y cómo deshacer errores.

## Material de respaldo
- Plan B sin internet: ejercicios 1 y .gitignore se hacen en local.
- Cheatsheet: `cheatsheet.md`.
