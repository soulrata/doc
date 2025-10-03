# 🚀 Guía de instalación de Laravel Boost en Docker + WSL

[Laravel Boost](https://github.com/laravel/boost) es un paquete pensado para mejorar la experiencia de desarrollo en proyectos Laravel, especialmente en entornos locales.
Si tu proyecto corre en **WSL + Docker**, puede que encuentres errores de permisos al instalarlo con Composer, debido a la caché compartida.

A continuación te presento **tres formas de instalación**, desde la más insegura (no recomendable en producción) hasta la más limpia.

---

## 1. 🔓 Habilitar todos los permisos (NO recomendado)

```bash
docker compose exec app chmod -R 777 /var/www/.composer/cache
docker compose exec app composer require laravel/boost --dev
docker compose exec app php artisan boost:install
```

* **Qué hace**: da permisos totales (`777`) a la carpeta de caché de Composer.
* **Ventaja**: rápido y funciona siempre.
* **Desventaja**: es inseguro y puede traer problemas si los volúmenes están montados desde el host.
* **Uso sugerido**: solo para pruebas rápidas en un entorno local totalmente descartable.

---

## 2. ⚡ Instalar sin usar la caché (seguro, pero más lento)

```bash
docker compose exec app composer require laravel/boost --dev --no-cache
docker compose exec app php artisan boost:install
```

* **Qué hace**: evita la caché de Composer y descarga todo desde cero.
* **Ventaja**: no necesitás tocar permisos ni configuraciones adicionales.
* **Desventaja**: cada instalación puede tardar más porque siempre baja dependencias nuevas.
* **Uso sugerido**: cuando solo querés instalar Boost una vez y no planeás hacer demasiados cambios de dependencias.

---

## 3. 🛡️ Configurar caché temporal (recomendado)

```bash
docker compose exec app composer config cache-dir /tmp/composer-cache
docker compose exec app composer require laravel/boost --dev
docker compose exec app php artisan boost:install
```

* **Qué hace**: cambia el directorio de caché de Composer a `/tmp/composer-cache`, que siempre es escribible dentro del contenedor.
* **Ventaja**: evita problemas de permisos sin abrir todo con `777`.
* **Desventaja**: la caché no se mantiene entre recreaciones de contenedores.
* **Uso sugerido**: la opción más balanceada y segura para desarrollo con Docker + WSL.

---

## ✅ Recomendación final

* Si querés algo rápido y no te importa la seguridad: **Opción 1**.
* Si querés algo seguro y no te importa la velocidad: **Opción 2**.
* Si querés el mejor balance entre seguridad y eficiencia: **Opción 3 (recomendada)**.

---

👉 Después de instalar, podés confirmar que Boost está funcionando corriendo:

```bash
docker compose exec app php artisan boost
```

Esto debería mostrarte los comandos disponibles de Boost.
