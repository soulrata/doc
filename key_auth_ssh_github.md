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

## 📌 En resumen

Ese bloque hace que tu computadora y GitHub/GitLab se reconozcan entre sí **usando criptografía SSH**.

* La **clave privada** queda en tu PC.
* La **clave pública** va en GitHub/GitLab.
* Cuando hacés `git push` o `git pull`, se autentican automáticamente sin pedir usuario/contraseña.
