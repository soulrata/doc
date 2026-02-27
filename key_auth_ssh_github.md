### 🔹 1) Generar clave SSH

```bash
ssh-keygen -t ed25519 -C "tu_email@empresa.com"
```

* Genera un **par de claves SSH**:

  * **Privada**: `~/.ssh/id_ed25519` → se guarda en tu PC (¡no la compartas!).
  * **Pública**: `~/.ssh/id_ed25519.pub` → esta sí la copias a GitHub/GitLab.
* `-t ed25519` → usa el algoritmo moderno y seguro Ed25519.
* `-C "email"` → comentario identificador (suele ser tu email).

👉 Resultado: un par de llaves que te sirven como **identidad segura**.

---

### 🔹 2) Levantar el agente SSH

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

* El **ssh-agent** es un programa que guarda en memoria tu clave privada.
* `ssh-add` carga la clave para no tener que escribir tu passphrase cada vez.
* Así, cada vez que Git quiera conectarse, el agente "firma" automáticamente con tu clave.

---

### 🔹 3) Copiar clave pública

```bash
cat ~/.ssh/id_ed25519.pub 
```

* Esto muestra tu **clave pública**.
* La copias y la pegas en GitHub/GitLab → Configuración → **SSH Keys**.
* A partir de ahí, GitHub/GitLab reconocen tu máquina como autorizada.

---

### 🔹 4) Probar la conexión

```bash
ssh -T git@github.com
# o
ssh -T git@gitlab.com
```

* Hace un intento de conexión vía SSH.
* Si todo salió bien, deberías ver algo como:

  ```
  Hi usuario! You've successfully authenticated, but GitHub does not provide shell access.
  ```
* Eso significa que **ya podés usar `git clone`, `git push`, `git pull` sin contraseñas**.

---

### 🔹 5) Escenario Multi-llave: Trabajar con diferentes proyectos/empresas

Cuando necesitas generar una **segunda clave SSH** (por ejemplo, para un proyecto freelance o una cuenta personal de GitHub), debes evitar "pisar" la clave anterior. El proceso es similar, pero con dos diferencias clave: **un nombre de archivo diferente** y el archivo de configuración `~/.ssh/config`.

#### A) Generar la nueva clave con un nombre específico
Ejecuta el siguiente comando, pero esta vez, cuando te pida la ubicación para guardar el archivo, **cámbiala** para no sobrescribir la `id_ed25519` anterior.

```bash
ssh-keygen -t ed25519 -C "tu_email_personal@personal.com"
```
**En la terminal verás:**
```bash
> Enter file in which to save the key (~/.ssh/id_ed25519):
```
👉 **Tienes que escribir** la nueva ruta manualmente:
```bash
~/.ssh/id_ed25519_personal
```
*(Puedes usar cualquier nombre descriptivo, como `id_ed25519_freelance`, `id_rsa_trabajo`, etc.)*

- Luego, te pedirá una **contraseña (passphrase)** de forma opcional (es recomendable poner una por seguridad).

#### B) Agregar la nueva clave al agente SSH
Al tener dos claves, debes agregar **ambas** al agente para que estén disponibles:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519       # La primera clave (trabajo)
ssh-add ~/.ssh/id_ed25519_personal # La segunda clave (personal)
```
*Puedes verificar que ambas estén cargadas con el comando `ssh-add -l`.*

#### C) Copiar la nueva clave pública a la otra cuenta
```bash
cat ~/.ssh/id_ed25519_personal.pub
```
Luego, copias el resultado y lo pegas en la **otra cuenta de GitHub/GitLab** en *Settings → SSH and GPG keys → New SSH key*.

---

### 🔹 6) El archivo mágico: `~/.ssh/config`

Ahora viene la parte más importante para que Git sepa **qué llave usar con cada proyecto o cuenta**. Debes crear (o editar) el archivo de configuración del cliente SSH.

Crea el archivo si no existe:
```bash
touch ~/.ssh/config
```
Luego, edítalo (con `nano`, `vim` o tu editor favorito):
```bash
nano ~/.ssh/config
```

Y agrega la siguiente estructura:

```bash
# Cuenta por defecto o del trabajo (usa la clave original)
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes

# Cuenta personal o secundaria (usa la nueva clave)
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes
```

**Explicación:**
- **Host**: Es un *alias* que inventas. Para la primera cuenta, dejamos `github.com` (el real). Para la segunda, usamos un alias como `github-personal`.
- **IdentityFile**: La ruta hacia la **clave privada** específica que debe usar.
- **IdentitiesOnly yes**: Le dice a SSH que *solo* intente con la llave que le especificas aquí, ignorando otras que pueda tener el agente (esto evita confusiones).

---

### 🔹 7) Clonar repositorios con el alias correcto

Aquí está el truco final. Cuando vayas a clonar un repositorio que pertenece a tu **cuenta personal**, **NO** debes usar la URL que te da GitHub por defecto (`git@github.com:usuario/proyecto.git`). Debes reemplazar `github.com` por el **alias** que definiste en el paso anterior.

**Por ejemplo:**
- Si el repo está en tu cuenta personal:
  ```bash
  # MAL (usaría la llave equivocada)
  git clone git@github.com:miUsuarioPersonal/proyecto.git

  # BIEN (usa el alias 'github-personal' del archivo config)
  git clone git@github-personal:miUsuarioPersonal/proyecto.git
  ```
- Si el repo está en tu cuenta de trabajo:
  ```bash
  # Usas el formato estándar, que ya apunta a la llave de trabajo
  git clone git@github.com:empresa/proyecto.git
  ```

**¿Qué pasa aquí?** Cuando escribes `git clone git@github-personal:...`, SSH busca en el archivo `~/.ssh/config` una entrada con el `Host github-personal`. Al encontrarla, sabe que debe conectarse al servidor real `github.com` (por la directiva `HostName`), pero usando la clave privada `~/.ssh/id_ed25519_personal`.

---

### 🔹 8) Verificar y gestionar

- **Ver qué claves están cargadas:**
  ```bash
  ssh-add -l
  ```
- **Eliminar todas las claves del agente (para empezar de nuevo):**
  ```bash
  ssh-add -D
  ```
- **Probar la conexión para cada alias:**
  ```bash
  ssh -T git@github.com          # Prueba la cuenta principal
  ssh -T git@github-personal      # Prueba la cuenta personal (debería saludar al otro usuario)
  ```

---

## 📌 Resumen del Escenario Multi-llave

| Paso | Acción |
| :--- | :--- |
| **1** | Generar la nueva clave con un nombre diferente (`id_ed25519_personal`). |
| **2** | Agregar **ambas** claves al agente SSH con `ssh-add`. |
| **3** | Copiar la nueva clave pública a la segunda cuenta en GitHub/GitLab. |
| **4** | **Crear/editar** el archivo `~/.ssh/config` para asignar cada llave a un "Host" distinto. |
| **5** | Al clonar, usar el **alias** correspondiente (`github-personal`) en lugar de `github.com`. |

Con este sistema, puedes tener tantas llaves como necesites, y Git/SSH siempre elegirá la correcta automáticamente.
