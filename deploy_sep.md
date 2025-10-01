# Guía para Desplegar SEP en Ubuntu Server 

---

### 1. Configurar el Servidor SSH (Putty)

---

### 2. Instalar Apache, PHP y Extensiones Necesarias

#### 2.1. Instalar Apache

1. **Actualizar los Paquetes:**
   ```bash
   sudo apt update
   ```

2. **Instalar Apache:**
   ```bash
   sudo apt install apache2 -y
   ```

3. **Verificar que Apache esté Corriendo:**
   ```bash
   sudo systemctl status apache2
   ```

---

#### 2.2. Instalar PHP 8.2 y Extensiones Necesarias

1. **Instalar los Repositorios Necesarios:**
   ```bash
   sudo apt install software-properties-common -y
   sudo add-apt-repository ppa:ondrej/php -y
   sudo apt update
   ```

2. **Instalar PHP 8.2 y las Extensiones Requeridas para Laravel:**
   ```bash
   sudo apt install php8.3 php8.3-cli php8.3-common php8.3-mysql php8.3-xml php8.3-mbstring php8.3-zip php8.3-curl php8.3-gd php8.3-bcmath php8.3-intl php8.3-fpm -y
   ```

3. **Reiniciar Apache y PHP-FPM:**
   ```bash
   sudo systemctl restart apache2
   sudo systemctl restart php8.3-fpm
   ```

4. **Verificar que PHP esté Correctamente Instalado:**
   ```bash
   php -v
   ```

---

### 3. Instalar MySQL

1. **Instalar MySQL:**
   ```bash
   sudo apt install mysql-server -y
   ```

2. **Verificar que el Servicio MySQL esté Corriendo:**
   ```bash
   sudo systemctl status mysql
   ```

3. **Ejecutar el Script de Seguridad:**
   ```bash
   sudo mysql_secure_installation
   ```

4. **Iniciar Sesión en MySQL:**
   ```bash
   sudo mysql
   ```

5. **Asignar una Contraseña al Usuario `root`:**
   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Rsp511***';
   FLUSH PRIVILEGES;
   EXIT;
   ```

6. **Verificar el Acceso con la Nueva Contraseña:**
   ```bash
   mysql -u root -p
   ```

7. **Crear la Base de Datos que Laravel Utilizará:**
   ```sql
   CREATE DATABASE reclutar;
   EXIT;
   ```

---

### 4. Clonar el Repositorio del Proyecto

1. **Navegar al Directorio donde Quieres Clonar el Proyecto (por ejemplo, `/var/www`):**
   ```bash
   cd /var/www
   ```

2. **Clonar el Repositorio desde GitHub:**
   ```bash
   sudo git clone <mi_repositorio>
   ```

3. **Cambiar los Permisos del Directorio del Proyecto:**
   ```bash
   sudo chown -R www-data:www-data /var/www/reclutar
   sudo chmod -R 775 /var/www/reclutar
   ```

---

### 5. Ajustar Permisos en Carpetas Críticas de Laravel

Laravel necesita permisos de escritura en las carpetas `storage` y `bootstrap/cache`.

1. **Ajustar los Permisos en las Carpetas Necesarias:**
   ```bash
   sudo chmod -R 775 /var/www/reclutar/storage
   sudo chmod -R 775 /var/www/reclutar/bootstrap/cache
   ```

---

### 6. Instalar Composer y Dependencias

1. **Instalar Composer:**
   ```bash
   sudo apt install composer -y
   ```

2. **Verificar la Instalación de Composer:**
   ```bash
   composer --version
   ```

3. **Navegar al Directorio del Proyecto Clonado:**
   ```bash
   cd /var/www/reclutar
   ```

4. **Ejecutar Composer como el Usuario `www-data` para Instalar las Dependencias:**
   ```bash
   sudo -u www-data composer install
   ```

---

### 7. Configurar el Archivo `.env`

1. **Cambiar la Propiedad del Directorio a tu Usuario (Opcional):**
   - Si el directorio `/var/www/reclutar` pertenece a `www-data`, puedes cambiar la propiedad a tu usuario:
     ```bash
     sudo chown -R admin:admin /var/www/reclutar
     ```
   - Esto permitirá hacer cambios sin `sudo`.

2. **Cambiar los Permisos de Escritura Temporalmente (Opcional):**
   ```bash
   sudo chmod u+w /var/www/reclutar
   ```

3. **Copiar el Archivo `.env.example`:**
   ```bash
   cp .env.example .env
   ```

4. **Editar el Archivo `.env` para Configurar la Base de Datos y Otras Configuraciones:**
   ```bash
   sudo nano .env
   ```
   - Asegúrate de cambiar los siguientes valores:
     ```
     DB_CONNECTION=mysql
     DB_HOST=127.0.0.1
     DB_PORT=3306
     DB_DATABASE=reclutar
     DB_USERNAME=root
     DB_PASSWORD=tu_contraseña
     ```

5. **Generar la Clave de la Aplicación:**
   ```bash
   php artisan key:generate
   ```

---

### 8. Configurar Apache para Laravel

1. **Crear un Archivo de Configuración de Apache para tu Proyecto:**
   ```bash
   sudo nano /etc/apache2/sites-available/reclutar.conf
   ```

2. **Añadir la Configuración del VirtualHost:**
   ```
   <VirtualHost *:80>
       ServerAdmin admin@reclutar.local
       ServerName reclutar.local
       DocumentRoot /var/www/reclutar/public

       <Directory /var/www/reclutar/public>
           Options Indexes FollowSymLinks
           AllowOverride All
           Require all granted
       </Directory>

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```

3. **Habilitar el Nuevo Sitio y el Módulo de Reescritura de Apache:**
   ```bash
   sudo a2ensite reclutar.conf
   sudo a2enmod rewrite
   sudo systemctl restart apache2
   ```

4. **Añadir una Entrada en el Archivo de Hosts:**
   ```bash
   sudo nano /etc/hosts
   ```
   - Añade la siguiente línea:
     ```
     127.0.0.1 reclutar.local
     ```

---

### 9. Instalar Dependencias de Frontend y Realizar Compilación

1. **Instalar Node.js y npm (si no lo tienes):**
   ```bash
   sudo apt install nodejs npm -y
   ```

2. **Instalar las Dependencias de `npm`:**
   ```bash
   npm install
   ```

3. **Compilar los Assets de Frontend:**
   ```bash
   npm run build
   ```

---

### 10. Migraciones de Base de Datos

1. **Ejecutar las Migraciones:**
   ```bash
   php artisan migrate
   ```

2. **(Opcional) Ejecutar los Seeders si Tienes Datos de Prueba:**
   ```bash
   php artisan db:seed
   ```

---

### Conclusión

Esta guía asegura que los permisos del sistema y las carpetas críticas estén configurados correctamente antes de ejecutar los comandos importantes como `composer install`. Con esta estructura, evitas errores comunes relacionados con permisos y configuraciones.
