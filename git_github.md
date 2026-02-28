# 📚 GUÍA COMPLETA DE GIT Y GITHUB (VERSIÓN EXTENDIDA CON FORMATO MEJORADO)

> [!NOTE]
> Esta guía está diseñada tanto para principiantes como para usuarios avanzados. Cada sección incluye ejemplos prácticos y soluciones a problemas comunes.

## 📋 ÍNDICE
1. [Configuración Inicial](#configuración-inicial-git-init-user-etc)
2. [Conceptos Básicos](#conceptos-básicos)
3. [Comandos Esenciales](#comandos-esenciales)
4. [Trabajo con Ramas](#trabajo-con-ramas)
5. [Pull Requests](#pull-requests)
6. [Problemas Comunes y Soluciones](#problemas-comunes)
7. [Comandos Avanzados Útiles](#comandos-avanzados)

---

## 🚀 CONFIGURACIÓN INICIAL (GIT INIT, USER, ETC)

> [!IMPORTANT]
> Esta configuración es **OBLIGATORIA** la primera vez que usas Git en tu computadora. Sin ella, no podrás hacer commits.

### 1️⃣ **Instalación de Git**

> [!TIP]
> Para verificar si ya tienes Git instalado, abre tu terminal y ejecuta: `git --version`

```bash
# Verificar si Git está instalado
git --version

# En Linux (Ubuntu/Debian)
sudo apt-get update
sudo apt-get install git

# En Mac
brew install git

# En Windows
# Descargar de https://git-scm.com/download/win
```

### 2️⃣ **Configuración Global de Usuario** (Hacer UNA SOLA VEZ)

> [!CAUTION]
> El email que configures DEBE ser el mismo que usas en GitHub, de lo contrario tus commits no se asociarán a tu cuenta.

```bash
# Configurar nombre de usuario (aparecerá en tus commits)
git config --global user.name "Tu Nombre Completo"

# Configurar email (DEBE ser el mismo que en GitHub)
git config --global user.email "tu-email@ejemplo.com"

# Verificar configuración
git config --global --list

# Configurar editor por defecto (opcional)
git config --global core.editor "code --wait"  # Para VS Code
git config --global core.editor "nano"         # Para Nano
git config --global core.editor "vim"          # Para Vim
```

### 3️⃣ **Configuración por Proyecto** (Por si quieres diferente identidad)

> [!NOTE]
> Útil cuando trabajas en múltiples proyectos con diferentes identidades (personal/laboral).

```bash
# Dentro de un proyecto específico
git config user.name "Nombre para este proyecto"
git config user.email "email-proyecto@ejemplo.com"
```

### 4️⃣ **Inicializar un Repositorio** (`git init`)

#### Opción A: Crear repositorio NUEVO local

> [!TIP]
> El comando `git init` crea una carpeta oculta `.git` que contiene todo el historial de tu proyecto. ¡No la borres!

```bash
# 1. Crear carpeta del proyecto
mkdir mi-proyecto
cd mi-proyecto

# 2. Inicializar Git
git init

# 3. Ver resultado (crea carpeta oculta .git)
ls -la  # En Linux/Mac
dir /a  # En Windows
```

#### Opción B: Clonar repositorio EXISTENTE de GitHub

> [!WARNING]
> Asegúrate de tener permisos de acceso al repositorio antes de clonar.

```bash
# Clonar con HTTPS
git clone https://github.com/usuario/repositorio.git

# Clonar con SSH (requiere configurar llaves)
git clone git@github.com:usuario/repositorio.git

# Clonar en carpeta específica
git clone https://github.com/usuario/repositorio.git mi-carpeta
```

### 5️⃣ **Configuración de SSH (Recomendado)** 🔐

> [!IMPORTANT]
> SSH es más seguro que HTTPS y no necesitas escribir tu usuario/contraseña cada vez.

```bash
# 1. Generar llave SSH
ssh-keygen -t ed25519 -C "tu-email@ejemplo.com"

# 2. Ver llave pública
cat ~/.ssh/id_ed25519.pub

# 3. Copiar llave y agregar en GitHub:
#    Settings → SSH and GPG keys → New SSH key

# 4. Probar conexión
ssh -T git@github.com
```

### 6️⃣ **Configuraciones Útiles Adicionales**

> [!TIP]
> Estas configuraciones mejoran tu experiencia con Git.

```bash
# Colores en la terminal (más legible)
git config --global color.ui auto

# Alias útiles (atajos)
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"

# Autocorrección (intenta adivinar comandos)
git config --global help.autocorrect 1

# Ignorar cambios en permisos de archivos
git config --global core.fileMode false

# Configurar saltos de línea (evita problemas Windows/Linux/Mac)
git config --global core.autocrlf input  # Para Mac/Linux
git config --global core.autocrlf true   # Para Windows
```

### 7️⃣ **Ver Configuraciones y Ayuda**

```bash
# Ver toda la configuración
git config --list

# Ver configuración global
git config --global --list

# Ver configuración local (del proyecto)
git config --local --list

# Ayuda sobre cualquier comando
git help config
git config --help
git help init
```

### 8️⃣ **.gitignore - Archivos a ignorar**

> [!CAUTION]
> NUNCA subas archivos con contraseñas, tokens o información sensible. El `.gitignore` es tu mejor aliado.

```bash
# Crear archivo .gitignore en la raíz del proyecto
touch .gitignore

# Ejemplo de .gitignore completo:
cat > .gitignore << EOF
# Dependencias
node_modules/
vendor/
package-lock.json

# Archivos de sistema
.DS_Store
Thumbs.db
desktop.ini

# Archivos de entorno
.env
.env.local
.env.*.local

# Logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Directorios de build
dist/
build/
out/

# IDE - Editor
.vscode/
.idea/
*.swp
*.swo
*~

# Archivos de test/coverage
coverage/
.nyc_output/

# Archivos comprimidos
*.zip
*.tar
*.gz

# Archivos de configuración sensibles
config/database.yml
config/secrets.yml
EOF
```

---

## 📦 FLUJO COMPLETO: DE 0 A GITHUB

### DÍA 1: Crear proyecto nuevo

> [!TIP]
> Este flujo es el estándar de la industria. ¡Apréndelo de memoria!

```bash
# 1. Configurar identidad (si no lo hiciste)
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# 2. Crear proyecto
mkdir mi-app
cd mi-app

# 3. Inicializar Git
git init

# 4. Crear archivos iniciales
echo "# Mi App" > README.md
echo "node_modules/" > .gitignore

# 5. Agregar y commitear
git add README.md .gitignore
git commit -m "chore: commit inicial"

# 6. Crear repositorio en GitHub (manualmente)
#    - Ir a github.com/new
#    - Crear repo SIN README (ya lo tienes)

# 7. Conectar local con GitHub
git remote add origin https://github.com/tuusuario/mi-app.git
# O con SSH: git remote add origin git@github.com:tuusuario/mi-app.git

# 8. Subir código
git branch -M main  # Renombrar rama a main si es necesario
git push -u origin main
```

### DÍA 2: Continuar trabajando

> [!NOTE]
> Siempre crea una nueva rama para cada funcionalidad. ¡Nunca trabajes directamente en main!

```bash
# 1. Obtener últimos cambios (si trabajas con otros)
git pull origin main

# 2. Crear rama para nueva feature
git checkout -b feature/nueva-funcionalidad

# 3. Hacer cambios
git add .
git commit -m "feat: agrega nueva funcionalidad"

# 4. Subir rama
git push -u origin feature/nueva-funcionalidad

# 5. En GitHub: Crear Pull Request
```

---

## 🎯 FLUJO CON EQUIPO: MÚLTIPLES USUARIOS

### Configuración por proyecto

> [!IMPORTANT]
> Cuando trabajas en varios proyectos con diferentes identidades, configura el usuario por proyecto para evitar confusiones.

```bash
# Proyecto A (trabajo)
cd /ruta/proyecto-a
git config user.name "Tu Nombre Profesional"
git config user.email "tu@empresa.com"

# Proyecto B (personal)
cd /ruta/proyecto-b
git config user.name "Tu Nombre Personal"
git config user.email "tu@gmail.com"

# Verificar configuración del proyecto actual
git config user.name
git config user.email
```

---

## 🆘 SOLUCIÓN DE PROBLEMAS COMUNES DE CONFIGURACIÓN

### 1️⃣ **"Please tell me who you are"**

> [!WARNING]
> Este error aparece cuando intentas hacer commit sin configurar tu identidad.

```bash
# Solución:
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

### 2️⃣ **"Permission denied (publickey)"**

> [!CAUTION]
> Error de autenticación SSH. No podrás subir código hasta resolverlo.

```bash
# Solución:
# 1. Verificar si tienes llave
ls -la ~/.ssh

# 2. Generar nueva llave
ssh-keygen -t ed25519 -C "tu@email.com"

# 3. Agregar a GitHub
cat ~/.ssh/id_ed25519.pub
# Copiar y pegar en GitHub Settings

# 4. Probar
ssh -T git@github.com
```

### 3️⃣ **Cambiar URL del remoto**

> [!TIP]
| Útil cuando cambias de HTTPS a SSH o viceversa.

```bash
# Ver URLs actuales
git remote -v

# Cambiar de HTTPS a SSH
git remote set-url origin git@github.com:usuario/repo.git

# Cambiar de SSH a HTTPS
git remote set-url origin https://github.com/usuario/repo.git
```

### 4️⃣ **Credenciales guardadas incorrectas**

```bash
# Limpiar credenciales guardadas
git config --global --unset credential.helper
git config --global credential.helper cache

# En Windows (Credential Manager)
# Buscar "Credential Manager" y eliminar entradas de GitHub
```

---

## 📋 CONFIGURACIONES AVANZADAS POR PROYECTO

### Diferentes usuarios según carpeta

> [!NOTE]
| Esta configuración avanzada automáticamente usa diferentes identidades según la carpeta donde trabajes.

```bash
# En tu ~/.gitconfig global, agrega:
[includeIf "gitdir:~/work/"]
    path = ~/.gitconfig-work
[includeIf "gitdir:~/personal/"]
    path = ~/.gitconfig-personal

# Luego crea ~/.gitconfig-work
[user]
    name = Tu Nombre Work
    email = tu@empresa.com

# Y ~/.gitconfig-personal
[user]
    name = Tu Nombre Personal
    email = tu@gmail.com
```

---

## 🚀 EL COMANDO QUE TE SALVÓ (AHORA CON CONTEXTO COMPLETO)

> [!IMPORTANT]
> **¡EL COMANDO MÁS IMPORTANTE DE ESTA GUÍA!** Cuando todo está desincronizado y no entiendes qué pasa:

```bash
# Cuando todo está desincronizado:
git fetch --all --prune

# Explicación completa:
# --all     : Actualiza TODAS las ramas remotas
# --prune   : Elimina referencias a ramas remotas que ya no existen
#            (como cuando alguien borró una rama en GitHub)

# Después de esto, puedes:
git status                    # Ver estado actual
git branch -a                 # Ver ramas actualizadas
git log --oneline --graph     # Ver historial real
```

> [!TIP]
| Este comando es como un "reset" de la comunicación entre tu local y GitHub. ¡Úsalo cuando todo parezca confuso!

---

## ✅ VERIFICACIÓN DE CONFIGURACIÓN

> [!NOTE]
| Ejecuta este script para verificar que todo está correctamente configurado:

```bash
# Script para verificar que todo está bien
echo "=== GIT VERSION ==="
git --version

echo -e "\n=== GIT CONFIGURACIÓN GLOBAL ==="
git config --global --list

echo -e "\n=== GIT CONFIGURACIÓN LOCAL (este proyecto) ==="
git config --local --list 2>/dev/null || echo "No hay config local"

echo -e "\n=== REMOTOS CONFIGURADOS ==="
git remote -v

echo -e "\n=== ÚLTIMO COMMIT ==="
git log -1 --pretty=format:"%h - %an, %ar : %s"
```

---

## 🎯 RESUMEN DE CONFIGURACIÓN INICIAL

> [!TIP]
| Guarda estos comandos en un lugar seguro. Son todo lo que necesitas para empezar:

```bash
# LO MÍNIMO NECESARIO PARA EMPEZAR:
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global color.ui auto

# EN CADA PROYECTO NUEVO:
git init
git remote add origin https://github.com/usuario/repo.git
git add .
git commit -m "Initial commit"
git branch -M main
git push -u origin main

# EN CADA PROYECTO EXISTENTE:
git clone https://github.com/usuario/repo.git
cd repo
# ¡Ya está! Todo configurado automáticamente
```

> [!CAUTION]
| Recuerda: **NUNCA** subas archivos con contraseñas, tokens o información sensible. Usa siempre `.gitignore` para excluirlos.

> [!IMPORTANT]
| Si algo sale mal, recuerda: `git fetch --all --prune` es tu mejor amigo. Este comando soluciona el 90% de los problemas de sincronización.
