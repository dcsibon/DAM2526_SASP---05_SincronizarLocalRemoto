
# **PRÁCTICA 5 — Sincronización del proyecto Local-Remoto**

## 🎯 **Objetivo de la práctica**

En esta práctica aprenderás a:

* Crear **dos repositorios en GitHub** para tu proyecto:

  * uno para las **fuentes** (Markdown + configuración que puedes llamar `SASP_Fase1_Src`),
  * otro para la **web generada** por MkDocs. Este repositorio ya debes tenerlo porque lo debías crear en la práctica 3 (se llamaba `SASP_Fase1_Web`)

* Sincronizar tu trabajo local con GitHub.
* Trabajar desde **varios ordenadores** (casa / instituto) sin perder cambios.
* Gestionar correctamente Git con `.gitignore`.

Al finalizar, podrás modificar tu web desde cualquier equipo y mantener todo actualizado.

---

## ✅ **Punto de partida (lo que ya tienes en tu PC)**

Tu carpeta del proyecto debe tener algo como:

```
saspfase1/
│── docs/
│── mkdocs.yml
│── venv/
└── site/
```

* `docs/` → aquí están los contenidos en **Markdown**
* `mkdocs.yml` → configuración del sitio
* `venv/` → entorno virtual de Python (NO se sube)
* `site/` → web generada por MkDocs (HTML, CSS, JS)

---

# ✅ **PARTE A — Crear y sincronizar el repositorio de FUENTES**

Este repositorio contendrá únicamente:

* `docs/`
* `mkdocs.yml`
* cualquier otro archivo fuente

### 1️⃣ Crear el repositorio vacío en GitHub

Nombre recomendado:

```
SASP_Fase1_Src
```

*(No crear README, ni nada adicional)*

---

### 2️⃣ En tu ordenador, entrar en el proyecto

```bash
cd saspfase1
```

---

### 3️⃣ Inicializar Git

```bash
git init
git status
```

Verás todos los archivos sin seguimiento.

---

### 4️⃣ Crear `.gitignore` para evitar subir carpetas innecesarias

```bash
nano .gitignore
```

Escribe dentro:

```
venv/
site/
```

Guarda y cierra.

Comprueba el efecto:

```bash
git status
```

Ahora **no aparecen** `venv/` ni `site/`.

---

### 5️⃣ Primer commit

```bash
git add .
git commit -m "Primer commit: fuentes MkDocs sin venv ni site"
```

---

### 6️⃣ Conectar con GitHub y subir *(HTTP + Token o SSH, lo que tú prefieras)*

```bash
git remote add origin <URL_DEL_REPO_SRC>
git branch -M main
git push -u origin main
```

✅ Ya tienes tu repositorio de fuentes funcionando.

---

# ✅ **PARTE B — Sincronizar la carpeta `site/` con el repositorio WEB**

Este repositorio ya existe en GitHub porque se subió manualmente antes.

Nombre recomendado:

```
SASP_Fase1_Web
```

El objetivo es que `site/` tenga **su propio control de versiones**, independiente del repositorio de fuentes.

---

## ✅ **Opción recomendada *(sin riesgos)*: Clonar dentro de `site/`**

### 1️⃣ Borrar o mover la carpeta `site` actual

Comprueba que estás en la carpeta `saspfase1` con el comando `pwd`. Si no necesitas guardar el contenido de la carpeta site, la eliminaremos:

```bash
rm -rf site
```

Si quieres conservarla:

```bash
mv site site_backup
```

---

### 2️⃣ Clonar el repositorio Web dentro de `site`

Comprueba que estás en la carpeta `saspfase1` con el comando `pwd`. Ahora clona el repo a la carpeta `site`. **IMPORTANTE** que en el comando le des el nombre de la carpeta, sino te ponga automáticamente el nombre del repositorio de Github *(aunque tampoco pasaría nada, porque podríamos renombrarlo)*

```bash
git clone <URL_DEL_REPO_WEB> site
```

Ahora `site/` ya está conectado con GitHub.

Comprueba:

```bash
cd site
git status
```

---

### 3️⃣ Regenerar la web y subir cambios

Cuando actualices contenido de los ficheros Markdown de tu web, es decir, los ficheros de la carpeta `docs` o la configuración de MkDocs en `mkdocs.yml`. Podrás trabajar de la siguiente manera:

```bash
cd ..

# Activar entorno virtual en Windows
.\venv\Scripts\activate

# Activar entorno virtual en Linux / macOS
source venv/bin/activate

# Generar la web
mkdocs build

cd site
git status
git add .
git commit -m "Nuevo despliegue de la web generada con MkDocs"
git push

cd ..
```

✅ El repositorio Web queda actualizado.

---

## ✅ **Opción alternativa (si ya versionaste `site/` antes)**

Si hiciste dentro de `site`:

```bash
git init
git add .
git commit …
git push
```

Aparecerá este error:

```
! [rejected] main -> main (fetch first)
```

Significa:

> El repositorio en GitHub tiene una historia distinta y no puede ser sobrescrita automáticamente.

### ✅ Si estás seguro de que quieres reemplazar lo que hay en GitHub

```bash
git push -u origin main --force
```

📌 **Nota:**
Este comando sobrescribe la rama en GitHub.
Solo debe usarse cuando se sabe que el contenido remoto puede eliminarse.

---

### ✅ **Nota: significado simple de `fetch`**

`git fetch` sirve para **traer cambios del remoto** sin aplicarlos todavía.

```bash
git fetch
```

Después puedes ver qué ha cambiado con:

```bash
git log origin/main
```

Si quieres integrar esos cambios en tu trabajo:

```bash
git merge origin/main
```

Esto es equivalente a:

```bash
git pull   # (fetch + merge al mismo tiempo)
```

`fetch` es útil cuando **no estás seguro** de si quieres mezclar todavía y prefieres mirar primero.

---

# ✅ **Flujo de trabajo a partir de ahora**

## 🔁 Cada vez que modifiques la web

1. Actualizar fuentes:

```bash
cd saspfase1
git pull
```

2. Editar `docs/` y/o `mkdocs.yml`
   
4. Recuerda siempre activar el entorno virtual que has creado para usar mkdocs:

```bash
# Activar entorno virtual en Windows
.\venv\Scripts\activate

# Activar entorno virtual en Linux / macOS
source venv/bin/activate
```

5. Probar:

```bash
mkdocs serve
```

4. Generar la web:

```bash
mkdocs build
```

5. Guardar fuentes:

```bash
git add .
git commit -m "Descripción del cambio"
git push
```

6. Subir la web:

```bash
cd site
git pull
git add .
git commit -m "Nuevo despliegue"
git push
cd ..
```

---

# ✅ **Trabajar en dos ordenadores (casa e instituto)**

### 🔹 Primera vez en el segundo ordenador

```bash
git clone <URL_SRC> saspfase1
cd saspfase1
git clone <URL_WEB> site
```

Crear entorno virtual e instalar MkDocs.

---

### 🔹 Cada vez que cambies de equipo

Antes de empezar:

```bash
cd saspfase1
git pull

cd site
git pull
cd ..
```

Al terminar:

```bash
# fuentes
git add .
git commit -m "Cambios realizados"
git push

# web
cd site
git add .
git commit -m "Nuevo despliegue"
git push
cd ..
```

---

# ✅ **Práctica completada**

Con esta estructura podrás:

✔ mantener tu proyecto sincronizado
✔ trabajar desde varios ordenadores
✔ publicar nuevas versiones sin errores
✔ entender mejor cómo funciona Git en proyectos reales

Si durante la práctica aparece algún aviso o error, **no borres nada**: revisa el mensaje y vuelve a ejecutar el comando indicado.
