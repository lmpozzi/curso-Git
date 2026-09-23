# Git — Cheat sheet

## Los 4 lugares donde vive tu trabajo

```
 DIRECTORIO DE          STAGING              REPO LOCAL            REPO REMOTO
 TRABAJO                (index)              (.git)                (GitHub)

 [ archivos ] ─ git add ─▶ [ staging ] ─ git commit ─▶ [ commits ] ─ git push ─▶ [ commits ]
                                                            ▲ ◀───── git fetch / git pull ────┘
                                          git clone: trae un repo entero desde el remoto
```

`git status`: te dice el estado actual del repositorio. **Ante cualquier duda, `git status`.**

## Comandos básicos de la terminal

En el curso usamos **Git Bash** (viene con Git para Windows); los mismos comandos sirven en Mac y Linux. Si abrís por error el CMD o PowerShell de Windows, en las otras columnas están los equivalentes.

En Git Bash el prompt te muestra dónde estás y en qué rama: `~/Desktop/mi-primer-repo (main)`.

### Moverse y mirar

| Qué quiero hacer                                                 | Git Bash / Linux / Mac | Windows CMD        | Windows PowerShell |
| ---------------------------------------------------------------- | ---------------------- | ------------------ | ------------------ |
| Ver en qué carpeta estoy                                         | `pwd`                  | `cd`               | `pwd`              |
| Listar archivos                                                  | `ls`                   | `dir`              | `ls`               |
| Listar con detalle **y archivos ocultos** (`.git`, `.gitignore`) | `ls -la`               | `dir /a`           | `ls -Force`        |
| Entrar a una carpeta                                             | `cd carpeta`           | `cd carpeta`       | `cd carpeta`       |
| Subir a la carpeta de arriba                                     | `cd ..`                | `cd ..`            | `cd ..`            |
| Ir a mi carpeta personal                                         | `cd ~`                 | `cd %USERPROFILE%` | `cd ~`             |
| Volver a la carpeta en la que estaba antes                       | `cd -`                 | —                  | —                  |
| Limpiar la pantalla                                              | `clear` (o Ctrl+L)     | `cls`              | `cls`              |

### Archivos y carpetas

| Qué quiero hacer                          | Git Bash / Linux / Mac       | Windows CMD                                            | Windows PowerShell           |
| ----------------------------------------- | ---------------------------- | ------------------------------------------------------ | ---------------------------- |
| Crear una carpeta                         | `mkdir carpeta`              | `mkdir carpeta`                                        | `mkdir carpeta`              |
| Crear un archivo vacío                    | `touch archivo.txt`          | `type nul > archivo.txt`                               | `New-Item archivo.txt`       |
| Ver el contenido de un archivo            | `cat archivo.txt`            | `type archivo.txt`                                     | `cat archivo.txt`            |
| Escribir una línea (**pisa** lo anterior) | `echo "hola" > archivo.txt`  | `echo hola> archivo.txt`                               | `echo "hola" > archivo.txt`  |
| Agregar una línea al final                | `echo "otra" >> archivo.txt` | `echo otra>> archivo.txt`                              | `echo "otra" >> archivo.txt` |
| Copiar un archivo                         | `cp origen destino`          | `copy origen destino`                                  | `cp origen destino`          |
| Copiar una carpeta entera                 | `cp -r carpeta copia`        | `xcopy /e /i carpeta copia`                            | `cp -r carpeta copia`        |
| Mover o renombrar                         | `mv viejo nuevo`             | `move viejo nuevo` (para renombrar: `ren viejo nuevo`) | `mv viejo nuevo`             |
| Borrar un archivo                         | `rm archivo.txt`             | `del archivo.txt`                                      | `rm archivo.txt`             |
| Borrar una carpeta con todo lo que tiene  | `rm -r carpeta`              | `rmdir /s carpeta`                                     | `rm -r carpeta`              |

⚠️ Lo que se borra desde la terminal **no pasa por la papelera**. Si el archivo ya estaba commiteado, Git todavía lo tiene; si no, se perdió.

### Abrir cosas

| Qué quiero hacer                                                   | Git Bash / Linux / Mac                                                                 | Windows CMD / PowerShell |
| ------------------------------------------------------------------ | -------------------------------------------------------------------------------------- | ------------------------ |
| Abrir la carpeta actual en VSCode                                  | `code .`                                                                               | `code .`                 |
| Abrir la carpeta actual en el explorador de archivos               | `explorer .` (Windows) · `open .` (Mac) · `xdg-open .` (Linux)                         | `explorer .`             |
| Abrir un archivo con su programa (ej.: una página en el navegador) | `start index.html` (Windows) · `open index.html` (Mac) · `xdg-open index.html` (Linux) | `start index.html`       |

### Rutas y atajos

- `.` es la carpeta actual, `..` la de arriba y `~` tu carpeta personal.
- En Git Bash las rutas usan `/` y los discos se escriben `/c/`, `/d/`: `C:\Users\ana\Desktop` es `/c/Users/ana/Desktop`. En CMD y PowerShell se usa `\`.
- Si una ruta tiene espacios, va entre comillas: `cd "Mis Documentos"`.
- **Tab** autocompleta nombres de archivos y carpetas. **↑ / ↓** recorren los comandos anteriores. **Ctrl+C** cancela lo que se está ejecutando.
- En Linux y Mac las mayúsculas importan: `README.md` y `readme.md` son archivos distintos.
- En PowerShell, `ls -la` da error (usá `ls -Force`) y `>` puede guardar el archivo con otra codificación de texto (UTF-16 o UTF-8 con BOM). Para el curso, usá Git Bash o creá los archivos desde VSCode.

## Configuración inicial (una sola vez)

```bash
git config --global user.name "Nombre Apellido"
git config --global user.email "tu@mail.com"
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --list
```

## Conexión con GitHub (SSH)

```bash
ssh-keygen -t ed25519 -C "tu@mail.com"     # genera el par de claves (una sola vez)
clip < ~/.ssh/id_ed25519.pub                # copia la llave PÚBLICA (Windows) · Mac: pbcopy < ...
ssh -T git@github.com                       # prueba la conexión con GitHub
git remote set-url origin git@github.com:usuario/repo.git   # cambiar un remoto de https a ssh
```

La clave **`.pub`** es la que va a GitHub (Settings → SSH and GPG keys). La **privada** (`id_ed25519`, sin `.pub`) **nunca se comparte ni se sube a un repo.** Guía completa: `guia-ssh.md`.

## Ciclo básico

| Comando                           | Qué hace                                            |
| --------------------------------- | --------------------------------------------------- |
| `git init`                        | Convierte la carpeta actual en un repo              |
| `git status`                      | Estado de los archivos                              |
| `git add <archivo>`               | Pasa un archivo a staging                           |
| `git add .`                       | Pasa **todo** a staging (usar con cuidado)          |
| `git commit -m "mensaje"`         | Guarda el staging como un commit                    |
| `git diff`                        | Cambios sin stagear                                 |
| `git diff --staged`               | Cambios ya stageados                                |
| `git log`                         | Historial completo                                  |
| `git log --oneline`               | Historial resumido                                  |
| `git log --oneline --graph --all` | Historial con forma de ramas                        |
| `git commit --amend`              | Corrige el **último** commit (mensaje y/o archivos) |
| `git commit --amend --no-edit`    | Igual, pero sin tocar el mensaje                    |

`--amend` no "edita" el commit: lo **reemplaza** por uno nuevo con otro hash. Sirve para corregir un typo en el mensaje o sumar un archivo que te olvidaste, siempre que sea el **último** commit. Regla de oro: no lo uses en un commit que **ya hiciste `push`**, salvo que sepas que nadie más lo bajó (si no, tu historial local y el remoto quedan distintos, como con `rebase`).

## `.gitignore`

Archivo en la raíz del repo con un patrón por línea. Ignorar: credenciales (`.env`), `node_modules/`, `build/`, `*.log`, archivos del sistema/editor.
Si un archivo **ya estaba trackeado**: `git rm --cached <archivo>` y recién ahí lo ignora.

[Documentación de gitignore](https://git-scm.com/docs/gitignore).

## Ramas

| Comando                    | Qué hace                                   |
| -------------------------- | ------------------------------------------ |
| `git branch`               | Lista las ramas (`-a` incluye las remotas) |
| `git branch <nombre>`      | Crea una rama                              |
| `git checkout <nombre>`    | Cambia de rama                             |
| `git checkout -b <nombre>` | Crea y cambia de rama                      |
| `git branch -d <nombre>`   | Borra una rama ya fusionada                |
| `git merge <rama>`         | Fusiona `<rama>` en la rama actual         |
| `git merge --abort`        | Cancela un merge con conflicto             |

**Conflicto:** Git marca el archivo así:

```
<<<<<<< HEAD
tu versión (la rama actual)
=======
la versión de la otra rama
>>>>>>> feature/x
```

Elegí qué queda, **borrá las tres marcas**, guardá, `git add <archivo>` y `git commit`.

**Comandos para resolver el conflicto, en orden:**

```bash
git status                       # te dice qué archivos están en conflicto ("both modified")
# abrí cada archivo en conflicto, elegí qué queda, borrá <<<<<<<, ======= y >>>>>>>
git add <archivo>                 # marca el archivo como resuelto (uno por uno, o todos con git add .)
git status                       # confirmá que ya no diga "Unmerged paths"
git commit                       # sin -m: se abre el editor con un mensaje ya armado por Git
```

Si te trabaste en el medio y querés volver atrás como si nada hubiera pasado: `git merge --abort`.

## Stash

| Comando                       | Qué hace                                            |
| ----------------------------- | --------------------------------------------------- |
| `git stash push -m "mensaje"` | Guarda cambios sin commitear y limpia el directorio |
| `git stash -u`                | Igual, incluyendo archivos nuevos sin trackear      |
| `git stash list`              | Lista los stashes                                   |
| `git stash pop`               | Recupera el último stash y lo borra de la lista     |

## Remotos

| Comando                       | Qué hace                                                                                  |
| ----------------------------- | ----------------------------------------------------------------------------------------- |
| `git remote add origin <url>` | Conecta tu repo local con uno remoto (URL SSH: `git@github.com:usuario/repo.git`)         |
| `git remote -v`               | Muestra los remotos                                                                       |
| `git push -u origin main`     | Sube `main` (la primera vez, con `-u`)                                                    |
| `git push`                    | Sube (después del primer `-u`)                                                            |
| `git fetch`                   | **Trae** novedades del remoto, sin tocar tu trabajo                                       |
| `git pull`                    | `fetch` + `merge`, de la rama remota que sigue tu rama actual                             |
| `git pull origin <rama>`      | Trae y mergea **esa** rama remota puntual en tu rama actual (aunque no sea la que seguís) |
| `git clone <url>`             | Descarga un repo completo                                                                 |

**Flujo con Pull Request:**

- Crear rama
- Hacer commits sobre esa rama
- Pushear a esa rama: `git push origin <rama>`
- Abrir PR en GitHub
- Revisión (resolución de conflictos)
- Merge
- `git checkout main`, `git pull`.

**Resolver un conflicto en un Pull Request:**

Suponiendo que el Pull Request es de `feature/rama-con-conflicto` a `main`:

- `git checkout feature/rama-con-conflicto`
- `git pull origin main`
- Resolver el conflicto en tu editor de texto
- `git add <archivos-resueltos>`
- `git commit -m "merge: resuelve los conflictos de..."`
- `git push origin feature/rama-con-conflicto`

## Conventional Commits

Formato: `tipo: descripción corta`

| Tipo       | Cuándo                                      |
| ---------- | ------------------------------------------- |
| `feat`     | Una funcionalidad nueva                     |
| `fix`      | Se arregla un error                         |
| `docs`     | Solo documentación                          |
| `refactor` | Cambiar el código sin cambiar lo que hace   |
| `chore`    | Tareas varias (configuración, dependencias) |

Ejemplos: `feat: agrega sección de contacto` · `fix: corrige tipeo en el título` · `docs: actualiza el README`
Más info: https://www.conventionalcommits.org/es/v1.0.0/

## Conventional Branch

Lo mismo que Conventional Commits, pero para el **nombre de la rama**: `tipo/descripción`.

| Tipo                   | Para qué                 |
| ---------------------- | ------------------------ |
| `feature/` (o `feat/`) | Una funcionalidad nueva  |
| `bugfix/` (o `fix/`)   | Corregir un error        |
| `hotfix/`              | Corrección urgente       |
| `release/`             | Preparar un lanzamiento  |
| `chore/`               | Tareas que no son código |

**Reglas de la descripción:** solo minúsculas, números y guiones (`-`); nada de mayúsculas, espacios ni `_`; sin guiones al principio, al final o dos seguidos. Las ramas troncales (`main`, `master`, `develop`) no llevan prefijo.

Válido: `feature/agrega-buscador` · `fix/header-roto` · `release/v1.2.0`
Inválido: `Feature/Nuevo Login` (mayúsculas y espacio) · `fix/header__bug` (guion bajo) · `feature/doble--guion`

Más info: https://conventionalbranch.org/

_(La spec también define prefijos para agentes de IA — `ai/`, `claude/`, `copilot/` — que no usamos en este curso, porque no trabajamos con asistentes.)_

## SOS: me confundí

| Situación                           | Qué hago                                               |
| ----------------------------------- | ------------------------------------------------------ |
| No sé qué pasa                      | `git status`                                           |
| "not a git repository"              | Estoy en la carpeta equivocada: `pwd`                  |
| Me quedé en pleno merge             | `git merge --abort`                                    |
| Cambié un archivo y me arrepiento   | `git restore <archivo>`                                |
| Hice `git add` de más               | `git restore --staged <archivo>`                       |
| Se abrió vim y no puedo salir       | `Esc` y luego `:q!` + Enter                            |
| `Permission denied (publickey)`     | GitHub no tiene tu llave pública: revisá `guia-ssh.md` |
| `push` rechazado (non-fast-forward) | `git pull` y volver a intentar                         |

## Para seguir aprendiendo

- Documentación oficial: https://git-scm.com/doc
- Libro _Pro Git_ (gratis, en español): https://git-scm.com/book/es/v2
- Oh My Git! (juego) · https://learngitbranching.js.org
- Extensiones de VSCode: **GitLens**, **Git Graph**, **GitHub Pull Requests**
