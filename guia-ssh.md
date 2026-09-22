# Guía: conectar tu compu con GitHub usando SSH

Para poder subir (`push`) y bajar (`pull`) cosas de GitHub, GitHub tiene que saber que **sos vos**. En este curso lo hacemos con **SSH**: una vez configurado, no te pide usuario ni contraseña nunca más.

## Cómo funciona (en 30 segundos)

Generás un **par de llaves**:

| Llave       | Archivo          | Qué es       | Dónde va                                                                |
| ----------- | ---------------- | ------------ | ----------------------------------------------------------------------- |
| **Pública** | `id_ed25519.pub` | El _candado_ | La subís a GitHub                                                       |
| **Privada** | `id_ed25519`     | La _llave_   | **Se queda en tu compu. Nunca la compartas ni la subas a ningún repo.** |

GitHub guarda tu candado; cuando te conectás, tu compu demuestra que tiene la llave que lo abre.

> Usá siempre **Git Bash** en Windows (o la terminal de Mac/Linux). Los comandos son iguales.

---

## Paso 1 — Fijate si ya tenés una llave

```bash
ls ~/.ssh
```

Si ves `id_ed25519` e `id_ed25519.pub`, ya tenés una: salteá al paso 3. Si no existe la carpeta o está vacía, seguí.

## Paso 2 — Generar la llave

```bash
ssh-keygen -t ed25519 -C "tu@mail.com"
```

Usá el mismo mail que en GitHub. Te va a hacer dos preguntas:

1. `Enter file in which to save the key` → apretá **Enter** (usa la ruta por defecto).
2. `Enter passphrase` → es una contraseña **opcional** que protege tu llave si alguien te roba la compu. Podés dejarla vacía (Enter, Enter) para el curso. Si la ponés, te la va a pedir en cada `push` salvo que uses el agente (ver "Extras" más abajo).

Al final vas a tener dos archivos: `id_ed25519` (privada) y `id_ed25519.pub` (pública).

## Paso 3 — Copiar la llave PÚBLICA

Ojo: siempre el archivo que termina en **`.pub`**.

- **Windows (Git Bash):** `clip < ~/.ssh/id_ed25519.pub`
- **Mac:** `pbcopy < ~/.ssh/id_ed25519.pub`
- **Linux:** `cat ~/.ssh/id_ed25519.pub` y copiar con el mouse todo el texto (o `xclip -selection clipboard < ~/.ssh/id_ed25519.pub`).

Es una sola línea que empieza con `ssh-ed25519` y termina con tu mail.

## Paso 4 — Agregarla a GitHub

1. En GitHub, click en tu foto (arriba a la derecha) → **Settings**.
2. En el menú de la izquierda: **SSH and GPG keys**.
3. Botón verde **New SSH key**.
4. **Title:** un nombre para reconocerla, por ejemplo `Notebook curso Git`.
5. **Key type:** `Authentication Key`.
6. **Key:** pegá lo que copiaste (Ctrl+V).
7. **Add SSH key** (puede pedirte tu contraseña o el código de 2FA de GitHub).

## Paso 5 — Probar la conexión

```bash
ssh -T git@github.com
```

La primera vez pregunta si confiás en el servidor:

```
The authenticity of host 'github.com (...)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Verificá que el fingerprint sea **igual** al de arriba (es el oficial de GitHub) y escribí **`yes`** completo.

Si todo salió bien:

```
Hi tu-usuario! You've successfully authenticated, but GitHub does not provide shell access.
```

Eso es **éxito** (GitHub no te da una terminal, solo confirma quién sos).

## Paso 6 — Usar direcciones SSH en tus repos

Las URLs de SSH tienen esta forma:

```
git@github.com:tu-usuario/mi-primer-repo.git
```

En la página del repo: botón verde **Code** → pestaña **SSH** → copiar.

Parado en el directorio de tu repositorio local:

```bash
git remote add origin git@github.com:tu-usuario/mi-primer-repo.git
```

Si ya tenías un remoto con la dirección `https://...` y te sigue pidiendo usuario y contraseña, cambiala:

```bash
git remote -v                                                   # mirá qué URL tiene
git remote set-url origin git@github.com:tu-usuario/mi-primer-repo.git
```

Pusheá tus cambios al remoto, la primera vez es con este comando:

```bash
git push -u origin main
```

---

## Reglas de oro

- **Nunca** compartas, mandes por mail ni subas a un repo el archivo `id_ed25519` (el que **no** termina en `.pub`). Es tu credencial, como una contraseña. Si se te escapa: borrá esa llave en GitHub (Settings → SSH and GPG keys → Delete) y generá otra.
- **Si usás una compu compartida o de laboratorio:** al terminar el curso, borrá la llave de GitHub y en la terminal ejecutá `rm ~/.ssh/id_ed25519 ~/.ssh/id_ed25519.pub`. Si no, la próxima persona que use esa compu puede entrar a tu cuenta.

## Problemas comunes

| Síntoma                                             | Qué pasó / qué hacer                                                                                                                                                       |
| --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Permission denied (publickey)`                     | GitHub no reconoce tu llave. Revisá que hayas pegado la **`.pub`** completa en GitHub (paso 4) y que estés usando el mismo usuario. Diagnóstico: `ssh -vT git@github.com`. |
| El comando se cuelga o dice `Connection timed out`  | La red bloquea el puerto 22. Solución de abajo (puerto 443).                                                                                                               |
| `Host key verification failed`                      | Escribiste otra cosa que `yes` en el paso 5. Volvé a ejecutar `ssh -T git@github.com`.                                                                                     |
| GitHub dice `Key is invalid` al pegarla             | Copiaste la privada, o copiaste incompleta. Tiene que ser **una sola línea que empieza con `ssh-ed25519`**.                                                                |
| Te sigue pidiendo usuario y contraseña              | El remoto es HTTPS. `git remote -v` y cambialo con `git remote set-url` (paso 6).                                                                                          |
| `WARNING: UNPROTECTED PRIVATE KEY FILE` (Mac/Linux) | `chmod 600 ~/.ssh/id_ed25519`                                                                                                                                              |

### Si la red bloquea el puerto 22

Algunas redes (colegios, empresas, laboratorios) bloquean SSH. GitHub permite usarlo por el puerto 443. Creá o editá el archivo `~/.ssh/config` (con VSCode: `code ~/.ssh/config`) y agregá:

```
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
```

Volvé a probar con `ssh -T git@github.com`. Las URLs de tus repos **no cambian**.

## Extras (opcional)

**Passphrase sin escribirla en cada push:** si le pusiste passphrase a tu llave, cargala una vez por sesión en el agente:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```
