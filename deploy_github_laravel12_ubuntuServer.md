# 📦 Guía Completa de Deploy de Proyecto Laravel 12 en Ubuntu Server 24.04 LTS

> **Objetivo**: Desplegar un proyecto Laravel 12 con stack moderno (Livewire, Tailwind CSS 4, Vite, etc.) en un servidor Ubuntu 24.04 LTS desde cero, mediante SSH, con buenas prácticas de seguridad, permisos y configuración.

---

## 🧰 Requisitos Previos

- Servidor Ubuntu 24.04 LTS instalado y accesible por SSH.
- Usuario con permisos `sudo`.
- Repositorio en GitHub con el proyecto Laravel 12 listo para producción.

---

## 🔁 Paso 0: Actualizar el sistema

### 📌 Comando
```bash
sudo apt update && sudo apt upgrade -y
```

### 📘 Explicación
Actualiza los paquetes del sistema operativo para asegurar compatibilidad y seguridad.

### ✅ Verificación
```bash
cat /etc/os-release | grep "PRETTY_NAME"
# Debe mostrar: Ubuntu 24.04 LTS
```

---

## 🐘 Paso 1: Instalar PHP 8.2+ con extensiones necesarias

### 📌 Comando
```bash
sudo apt install -y software-properties-common
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
sudo apt install -y php8.3 php8.3-cli php8.3-fpm php8.3-mbstring php8.3-xml php8.3-curl php8.3-mysql php8.3-zip php8.3-bcmath php8.3-gd php8.3-redis php8.3-opcache
```

> Laravel 12 requiere **PHP ≥ 8.2**. Usamos PHP 8.3 por ser la más estable en 2025.

### 📘 Explicación
Instalamos PHP con todas las extensiones requeridas por Laravel, Livewire, Spatie Permissions, etc.

### ✅ Verificación
```bash
php -v
# Debe mostrar PHP 8.3.x

php -m | grep -E "mbstring|xml|curl|mysql|zip|bcmath|gd|redis"
# Debe listar todas las extensiones sin errores
```

---

## 🗄️ Paso 2: Instalar y configurar MySQL

### 📌 Comando
```bash
sudo apt install -y mysql-server
sudo mysql_secure_installation
```

> Sigue las instrucciones interactivas (recomendado: desactivar login anónimo, deshabilitar root remoto, eliminar DB de prueba).

### 📘 Explicación
MySQL es la base de datos principal. `mysql_secure_installation` mejora la seguridad.

### ✅ Verificación
```bash
sudo systemctl status mysql
# Debe estar "active (running)"
```

### 🔐 Crear base de datos y usuario
```bash
sudo mysql -e "CREATE DATABASE tu_proyecto CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
sudo mysql -e "CREATE USER 'tu_usuario'@'localhost' IDENTIFIED BY 'tu_contraseña_segura';"
sudo mysql -e "GRANT ALL PRIVILEGES ON tu_proyecto.* TO 'tu_usuario'@'localhost';"
sudo mysql -e "FLUSH PRIVILEGES;"
```

> Reemplaza `tu_proyecto`, `tu_usuario` y `tu_contraseña_segura` por valores reales.

---

## 📦 Paso 3: Instalar Composer 2.x

### 📌 Comando
```bash
cd /tmp
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

### 📘 Explicación
Composer es el gestor de dependencias de PHP. Laravel lo requiere para instalar paquetes como Livewire, Fortify, Spatie, etc.

### ✅ Verificación
```bash
composer --version
# Debe mostrar Composer 2.x
```

---

## 🟢 Paso 4: Instalar Node.js 20+ y NPM 10+

### 📌 Comando
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

### 📘 Explicación
Node.js y NPM son necesarios para compilar assets con Vite y Tailwind CSS 4.

### ✅ Verificación
```bash
node -v  # ≥ v20.x
npm -v   # ≥ 10.x
```

---

## ⚙️ Paso 5: Instalar y configurar Nginx

### 📌 Comando
```bash
sudo apt install -y nginx
sudo ufw allow 'Nginx Full'
```

### 📘 Explicación
Nginx actuará como servidor web. Es ligero, rápido y compatible con Laravel.

### ✅ Verificación
```bash
sudo systemctl status nginx
# Debe estar activo

curl -I http://localhost
# Debe devolver HTTP/1.1 200 OK
```

---

## 🧠 Paso 6: Instalar Redis (opcional pero recomendado para Laravel)

### 📌 Comando
```bash
sudo apt install -y redis-server
sudo systemctl enable redis-server
sudo systemctl start redis-server
```

### 📘 Explicación
Redis mejora el rendimiento de Laravel (caché, queues, sessions). Livewire y Fortify lo usan internamente.

### ✅ Verificación
```bash
redis-cli ping
# Debe responder: PONG
```

---

## 📂 Paso 7: Crear estructura de directorios y clonar el repositorio

### 📌 Comando
```bash
sudo mkdir -p /var/www/html/tu_proyecto
sudo chown -R $USER:$USER /var/www/html/tu_proyecto
cd /var/www/html/tu_proyecto
git clone https://github.com/tu_usuario/tu_repositorio.git .
```

> Reemplaza la URL por tu repositorio real.

### 📘 Explicación
Clonamos el proyecto en `/var/www/html/tu_proyecto`. Usamos el usuario actual (no root) para evitar problemas de permisos.

### ✅ Verificación
```bash
ls -la
# Debe mostrar los archivos de Laravel (artisan, composer.json, etc.)
```

---

## 🔧 Paso 8: Instalar dependencias de PHP y Node

### 📌 Comando
```bash
composer install --optimize-autoloader --no-dev
npm ci
```

> Usamos `--no-dev` en producción. `npm ci` asegura coherencia con `package-lock.json`.

### 📘 Explicación
- `composer install` instala paquetes como Livewire, Fortify, Spatie, etc.
- `npm ci` instala dependencias exactas del `package-lock.json` (Vite, Tailwind CSS 4, etc.).

### ✅ Verificación
```bash
ls vendor/          # Debe existir
ls node_modules/    # Debe existir
```

---

## 🛠️ Paso 9: Configurar entorno y base de datos

### 📌 Comando
```bash
cp .env.example .env
nano .env
```

### 📘 Explicación
Edita `.env` con los valores reales:

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=http://tu_dominio_o_ip

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=tu_proyecto
DB_USERNAME=tu_usuario
DB_PASSWORD=tu_contraseña_segura

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379
```

> **Importante**: `APP_DEBUG=false` en producción.

### ✅ Verificación
```bash
grep APP_ENV .env    # Debe ser "production"
grep DB_DATABASE .env # Debe coincidir con la BD creada
```

---

## 🔐 Paso 10: Generar clave de aplicación y ejecutar migraciones

### 📌 Comando
```bash
php artisan key:generate
php artisan migrate --force
```

> `--force` es necesario en entorno `production`.

### 📘 Explicación
- `key:generate` crea la clave de cifrado de la app.
- `migrate` crea las tablas (incluyendo las de Spatie Permissions y Fortify).

### ✅ Verificación
```bash
grep APP_KEY .env
# Debe contener una clave aleatoria

mysql -u tu_usuario -p -e "USE tu_proyecto; SHOW TABLES;"
# Debe listar tablas como users, permissions, roles, etc.
```

---

## 🎨 Paso 11: Compilar assets frontend (Tailwind CSS + Vite)

### 📌 Comando
```bash
npm run build
```

### 📘 Explicación
Compila los assets con Vite usando la configuración de `vite.config.js` y Tailwind CSS 4.

### ✅ Verificación
```bash
ls public/build/
# Debe contener assets como assets/app-*.js y app-*.css
```

---

## 🔐 Paso 12: Configurar permisos de carpetas (¡CRÍTICO!)

### 📌 Comando
```bash
sudo chown -R www-data:www-data /var/www/html/tu_proyecto
sudo chmod -R 755 /var/www/html/tu_proyecto
sudo chmod -R 775 /var/www/html/tu_proyecto/storage
sudo chmod -R 775 /var/www/html/tu_proyecto/bootstrap/cache
```

### 📘 Explicación
- `www-data` es el usuario de Nginx y PHP-FPM.
- `storage/` y `bootstrap/cache` deben ser **escribibles** por el servidor web.
- El resto del proyecto debe ser legible pero no modificable.

> ✅ **Buena práctica**: Nunca usar `777`.

### ✅ Verificación
```bash
ls -ld storage bootstrap/cache
# Debe mostrar: drwxrwxr-x ... www-data www-data
```

---

## 🌐 Paso 13: Configurar Nginx

### 📌 Comando
```bash
sudo nano /etc/nginx/sites-available/tu_proyecto
```

Pega esta configuración:

```nginx
server {
    listen 80;
    server_name tu_dominio_o_ip;
    root /var/www/html/tu_proyecto/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

Activa el sitio:
```bash
sudo ln -s /etc/nginx/sites-available/tu_proyecto /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### 📘 Explicación
Configura Nginx para servir Laravel desde `/public`, con seguridad y soporte PHP-FPM.

### ✅ Verificación
```bash
sudo nginx -t
# Debe decir: syntax is ok, test is successful
```

---

## 🔄 Paso 14: Reiniciar servicios y verificar

### 📌 Comando
```bash
sudo systemctl restart nginx php8.3-fpm
```

### ✅ Verificación final
Abre en tu navegador:
```
http://tu_ip_o_dominio
```

> Debe cargar tu app Laravel sin errores.

También verifica por CLI:
```bash
curl -s http://localhost | head -n 5
# Debe mostrar HTML de tu app
```

---

## 🧪 (Opcional) Paso 15: Configurar Supervisor para queues (si usas jobs)

Si tu app usa colas (queues), instala Supervisor:
```bash
sudo apt install -y supervisor
```

Crea un archivo de configuración en `/etc/supervisor/conf.d/laravel-worker.conf`:
```bash
sudo nano /etc/supervisor/conf.d/laravel-worker.conf
```

Luego pega tu configuración:
```ini
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/html/tu_proyecto/artisan queue:work --sleep=3 --tries=3
autostart=true
autorestart=true
user=www-data
numprocs=2
redirect_stderr=true
stdout_logfile=/var/www/html/tu_proyecto/storage/logs/worker.log
```

Guarda y sal:
- Presiona `Ctrl + O` → Enter (para guardar)
- Presiona `Ctrl + X` (para salir)

> ⚠️ Asegúrate de que la carpeta de logs exista y tenga permisos adecuados:
```bash
sudo mkdir -p /var/www/html/tu_proyecto/storage/logs
sudo chown -R www-data:www-data /var/www/html/tu_proyecto/storage
```

Luego:
```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start laravel-worker:*
```

---

## 📌 Nota importante sobre permisos

- Nunca edites archivos en `/etc/` sin `sudo` si no eres root.
- El usuario `www-data` (usado en `user=www-data`) debe tener permisos de lectura y escritura en:
  - El proyecto (al menos en `storage/` y `bootstrap/cache`)
  - El archivo de log (`worker.log`)

---

## 🎉 ¡Listo! Tu app Laravel 12 está desplegada con buenas prácticas.

✅ Stack completo funcionando:  
- Backend: Laravel 12, Livewire 3, Volt, Fortify, Spatie Permissions  
- Frontend: Tailwind CSS 4, Alpine.js, Chart.js, Flux UI  
- Infra: Nginx, PHP 8.3, MySQL, Redis, Node.js 20  

🔒 Seguridad: permisos correctos, debug desactivado, headers de seguridad en Nginx.

---

> 📝 **Nota**: Para producción real, considera agregar:
> - Certificado SSL con Let's Encrypt (`certbot`)
> - Firewall (`ufw`)
> - Backups automáticos de base de datos
> - Monitoreo (como `fail2ban`)




