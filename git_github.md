# 📘 Manual de Git para Principiantes

Este manual resume los comandos más usados en **Git y GitHub**, con ejemplos prácticos y buenas prácticas.

---

## 🔹 1. Configuración inicial de Git

Antes de usar Git, configurá tu nombre y correo (se usan en los commits):

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tuemail@ejemplo.com"
```

Ver tu configuración actual:

```bash
git config --list
```

---

## 🔹 2. Crear o clonar un repositorio

### Crear un repositorio nuevo

```bash
git init
```

### Clonar un repositorio existente (desde GitHub)

```bash
git clone https://github.com/usuario/repositorio.git
```

---

## 🔹 3. Comandos básicos de trabajo

### Ver estado del repo

```bash
git status
```

### Agregar archivos al área de preparación

```bash
git add archivo.txt
git add .   # agrega todos los cambios
```

### Guardar cambios (commit)

```bash
git commit -m "Mensaje claro y descriptivo"
```

### Subir cambios al remoto (GitHub)

```bash
git push origin rama
```

### Traer cambios desde remoto

```bash
git pull origin rama
```

---

## 🔹 4. Manejo de ramas

### Ver ramas disponibles

```bash
git branch
```

### Crear una nueva rama

```bash
git branch nombre-rama
```

### Cambiar de rama

```bash
git checkout nombre-rama
```

### Crear y cambiar a la nueva rama en un paso

```bash
git checkout -b feature/login
```

### Subir la rama al remoto (GitHub)

```bash
git push -u origin feature/login
```

### Eliminar rama local

```bash
git branch -d nombre-rama
```

### Eliminar rama remota

```bash
git push origin --delete nombre-rama
```

---

## 🔹 5. Pull Requests (PR)

### Flujo típico

1. Crear rama nueva:

   ```bash
   git checkout -b feature/nueva-funcion
   ```
2. Hacer cambios y confirmarlos:

   ```bash
   git add .
   git commit -m "Implemento nueva función"
   ```
3. Subir rama:

   ```bash
   git push origin feature/nueva-funcion
   ```
4. Ir a GitHub → pestaña **Pull requests** → **New pull request**.

   * Base branch: `dev` o `main`
   * Compare branch: `feature/nueva-funcion`
5. Describir los cambios y crear el PR.

---

## 🔹 6. Fusionar ramas (Merge)

### Hacer merge de una rama en otra

1. Cambiarte a la rama destino:

   ```bash
   git checkout dev
   ```
2. Traer cambios de otra rama:

   ```bash
   git merge feature/nueva-funcion
   ```

### Resolver conflictos

* Si hay conflictos, Git marcará las diferencias en los archivos.
* Editar los archivos y elegir qué código conservar.
* Luego:

  ```bash
  git add archivo.txt
  git commit -m "Resuelvo conflictos en archivo.txt"
  ```

---

## 🔹 7. Buenas prácticas

✔️ Usa **mensajes de commit claros y cortos**.
✔️ Trabajá en ramas separadas (`feature/`, `fix/`, `hotfix/`).
✔️ Sincronizá seguido con `git pull` para evitar conflictos grandes.
✔️ Antes de hacer un merge a `main`, revisá y probá bien el código.
✔️ Eliminá ramas que ya no uses para mantener limpio el repo.
✔️ Usá `.gitignore` para evitar subir archivos innecesarios (ejemplo: `node_modules/`, `.env`).

---

## 🔹 8. Resumen de comandos más usados

```bash
# Configuración
git config --global user.name "Nombre"
git config --global user.email "correo"

# Repositorio
git init
git clone URL

# Cambios
git status
git add .
git commit -m "mensaje"
git push origin rama
git pull origin rama

# Ramas
git branch
git checkout nombre-rama
git checkout -b nueva-rama
git merge otra-rama
git branch -d rama
git push origin --delete rama
```

---