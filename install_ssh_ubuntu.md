# Guía para Configurar el Servidor SSH en Ubuntu

Esta guía proporciona los pasos necesarios para configurar un servidor SSH en Ubuntu.

1. Inicia sesión en tu servidor **Ubuntu** y actualiza los paquetes:
    ```bash
    sudo apt update
    ```

2. Instala el paquete del servidor SSH:
    ```bash
    sudo apt install openssh-server -y
    ```

3. Verifica que el servicio SSH esté funcionando:
    ```bash
    sudo systemctl status ssh
    ```

4. Habilita el servicio SSH para que inicie automáticamente:
    ```bash
    sudo systemctl enable ssh
    ```

5. Inicia el servicio SSH manualmente si no está corriendo:
    ```bash
    sudo systemctl start ssh
    ```

6. Verifica que el puerto SSH esté permitido en el firewall:
    ```bash
    sudo ufw allow OpenSSH
    ```

> [!TIP]
> Asegúrate de tener el puerto SSH habilitado en el firewall para evitar problemas de conexión remota.
