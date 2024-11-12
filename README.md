# Guía para configurar un VPS en Digital Ocean (Virtual Personal Server)

## **1. Acceder al servidor**
```bash
# Acceso por contraseña (te pedirá la contraseña al presionar Enter)
ssh root@127.0.0.1

# Acceso por SSH
ssh root@127.0.0.1 -i c:/user/.ssh/private_key_ssh # Mas información en ssh/connect-to-vps-by-ssh.md
```

## **2. Actualizar paquetes del sistema**
Actualiza los paquetes del sistema al acceder por primera vez al VPS.
```bash
sudo apt update && sudo apt upgrade -y
```
* **apt update** actualiza la lista de paquetes disponibles en los repositorios configurados.
* **apt upgrade** instala actualizaciones de los paquetes existentes.
* La opción **-y** responde automáticamente "yes" a las solicitudes de confirmación.

## **3. Crear un usuario con permisos SUDO**
Por seguridad, es recomendable crear un nuevo usuario con permisos `sudo` y deshabilitar el usuario `root` posteriormente.
```bash
# 1 Crear al usuario
adduser user_angel

# 2 Darle permisos sudo 
usermod -aG sudo user_angel # Esto asigna al usuario `user_angel` al grupo `sudo`, dándole los mismos privilegios que `root`.
```

## **4 Acceder con nuevos usuarios mediante llaves SSH**
Sigue estos pasos para configurar el acceso mediante llaves ssh:

1. Genera las llaves SSH en tu máquina:
   ```bash
   ssh-keygen -t rsa
   ```

2. Accede al servidor con `root`.
3. Navega al directorio del nuevo usuario:
   ```bash
   cd /home/user_angel
   ```

4. Crea la carpeta `.ssh` y el archivo `authorized_keys` si no existen:
   ```bash
   mkdir -p .ssh && nano .ssh/authorized_keys
   ```

5. Copia el contenido de tu archivo `.pub` local en `authorized_keys` y guarda:
   ```bash
   ctrl + o  # Guardar archivo
   enter     # Confirmar
   ctrl + x  # Salir de nano
   ```

6. Prueba la conexión:
   ```bash
   ssh user_angel@127.0.0.1 -i c:/user/.ssh/private_key_ssh
   ```

7. Facilita la autenticación creando un archivo `config` en `~/.ssh` en tu maquina local:
   ```bash
   nano ~/.ssh/config
   ```
   Pega lo siguiente:
   ```config
   ServerAliveInterval 120
   ServerAliveCountMax 3

   Host my_server_user_angel
       HostName 127.0.0.1
       User uAngel
       IdentityFile c:/user/.ssh/private_key_ssh_uAngel
   ```

8. Ahora, solo necesitas usar:
   ```bash
   ssh my_server_user_angel
   ```

## **5. Deshabilitar el usuario ROOT**
Inicia sesión con un usuario con permisos `sudo` y navega a la configuración de SSH:
```bash
cd /etc/ssh
sudo nano sshd_config
```

Busca y modifica:
```bash
PermitRootLogin off  # Deshabilitar inicio de sesión con root
```

Guarda y recarga SSH:
```bash
sudo systemctl reload ssh.service
```

Verifica el acceso de `root`:
```bash
# Comprobar si el usuario root está deshabilitado
sudo su
```

## **6. UFW (Firewall)**
Configura UFW para proteger el tráfico entrante.
> **NOTA**: Permite conexiones SSH antes de habilitar UFW para evitar perder el acceso.

1. Verifica el estado de UFW:
   ```bash
   sudo ufw status
   ```

2. Lista las aplicaciones:
   ```bash
   sudo ufw app list
   ```

3. Permite conexiones SSH:
   ```bash
   sudo ufw allow "OpenSSH"
   ```

4. Activa UFW:
   ```bash
   sudo ufw enable
   ```

## **7. Fail2Ban**
Protege el servidor de intentos de acceso no autorizados.

1. Instala Fail2Ban:
   ```bash
   sudo apt update && sudo apt install fail2ban
   ```

2. Navega al directorio de configuración:
   ```bash
   cd /etc/fail2ban
   ```

3. Crea el archivo de configuración:
   ```bash
   sudo nano jail.local
   ```
   Contenido del archivo:
   ```ini
   [DEFAULT]
   bantime = 3h
   maxretry = 3

   [sshd]
   enabled = true
   ```

4. Recarga Fail2Ban:
   ```bash
   sudo fail2ban-client status
   sudo systemctl restart fail2ban.service
   ```

¡Listo! Con esta guía, tu VPS estará más seguro y configurado de manera eficiente.
