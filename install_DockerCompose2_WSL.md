# Guía: Instalar Docker y Docker Compose V2 en Ubuntu sobre WSL (solo consola)

Esta guía te permite instalar Docker y el plugin Compose V2 para usar el comando moderno:
```bash
docker compose up -d --build
```
sin depender del clásico `docker-compose`.

---

## 1. Instalar Docker Engine

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y ca-certificates curl gnupg lsb-release
```

Agregar la clave GPG y el repositorio oficial:
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Instalar Docker Engine y CLI:
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

---

## 2. Instalar Docker Compose V2 (plugin oficial)

Desde 2022, Docker Compose V2 viene como **plugin** y se usa así:  
`docker compose ...` (no más `docker-compose ...`).

Instala el paquete del plugin:
```bash
sudo apt install -y docker-compose-plugin
```

---

## 3. Permitir usar Docker sin sudo

```bash
sudo usermod -aG docker $USER
```
Cierra la terminal y vuelve a abrirla para que se aplique el grupo.

Verifica con:
```bash
groups
```
Debe aparecer `docker`.

---

## 4. Verifica versiones

```bash
docker --version
docker compose version
```
La salida de `docker compose version` debe mostrar algo como `Docker Compose version v2.x.x`.

---

## 5. Prueba el comando moderno

Coloca tu archivo `docker-compose.yml` en el directorio de tu proyecto y ejecuta:
```bash
docker compose up -d --build
```

---

## 6. Comandos útiles con Docker Compose V2

- Levantar servicios en segundo plano:
  ```bash
  docker compose up -d
  ```
- Reconstruir imágenes y levantar:
  ```bash
  docker compose up -d --build
  ```
- Ver logs:
  ```bash
  docker compose logs -f
  ```
- Parar y eliminar servicios:
  ```bash
  docker compose down
  ```

---

## 7. Recursos

- [Instalación oficial Compose V2](https://docs.docker.com/compose/install/linux/)
- [Guía rápida Docker Compose](https://docs.docker.com/compose/)
- [Manual Docker por consola (español)](https://github.com/Reclutar/Manuales/blob/main/docker-por-consola.md)

---

¡Listo! Ahora puedes usar el comando moderno `docker compose up -d --build` nativamente en Ubuntu WSL.
