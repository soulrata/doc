# 📘 Guía básica de Git y GitHub para el equipo

## 1. Clonar el repositorio
Para traer el repositorio de GitHub a tu computadora:
```bash
git clone https://github.com/usuario/nombre-del-repo.git
cd nombre-del-repo
```

---

## 2. Crear y cambiar a una nueva rama
Siempre trabajamos en ramas distintas a `main`.

### Crear una rama nueva desde main
```bash
git checkout -b feature/nombre-tarea
```
Ejemplo:
```bash
git checkout -b feature/formulario-login
```

### Cambiar a una rama existente en tu computadora
```bash
git checkout feature/formulario-login
```

### Cambiar a una rama que existe en GitHub pero no en tu compu
```bash
git checkout -b feature/formulario-login origin/feature/formulario-login
```

---

## 3. Ver estado de cambios
```bash
git status
```

---

## 4. Agregar cambios al área de preparación
```bash
git add .
```
(O podés agregar archivos puntuales: `git add archivo.php`).

---

## 5. Hacer un commit
Usar prefijos claros como `feat`, `fix`, `docs`, `refactor`, `chore`.

```bash
git commit -m "feat: agregar formulario de login"
```

---

## 6. Subir cambios a GitHub
```bash
git push origin nombre-de-tu-rama
```

Ejemplo:
```bash
git push origin feature/formulario-login
```

---

## 7. Actualizar tu rama local (traer cambios de `main`)
Primero asegurate de estar en `main`:
```bash
git checkout main
git pull origin main
```

Luego volvé a tu rama y actualizala con los cambios de `main`:
```bash
git checkout feature/formulario-login
git merge main
```

(Si hay conflictos, resolverlos antes de continuar).

---

## 8. Crear un Pull Request (PR)
1. Ir a GitHub → al repo.  
2. Vas a ver un botón: **“Compare & pull request”**.  
3. Elegir:
   - **Base branch:** `main`  
   - **Compare branch:** tu rama (`feature/...` , `release/...` , `hotfix/...`).  
4. Agregar título y descripción.  
5. Crear el PR.  

👉 Ahora tus compañeros revisan y aprueban.

---

## 9. Merge de un Pull Request
Cuando el PR está aprobado:
1. En GitHub → botón **“Merge pull request”**.  
2. Confirmar con **“Confirm merge”**.  
3. Borrar la rama (opcional, para mantener limpio el repo).

---

## 10. Actualizar tu repo después de un merge
Volver a `main` y traer los últimos cambios:
```bash
git checkout main
git pull origin main
```

---

# ✅ Convenciones de commits
Usamos estos prefijos:

- `feat:` → nueva funcionalidad  
- `fix:` → corrección de bug  
- `docs:` → cambios en documentación  
- `refactor:` → reestructuración sin cambiar lógica  
- `chore:` → tareas menores/configuración  
- `test:` → cambios o agregado de pruebas  

Ejemplo:
```
feat: implementar dashboard de usuario
fix: corregir validación de contraseña
docs: agregar pasos de instalación al README
```
