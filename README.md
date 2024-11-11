# Guia para configurar un VPS en digital Ocean (Virtual personal Server)

## **1. Crear el VPS**
Nada del otro mundo, creas el VPS eliges los componentes que necesite tu servidor y por ultimo tienes que escoger porque que metodo se van a loguear el usuario **ROOT** ( **PASSWORD** | **SSH** ). 
* **PASSWORD**: Creas un password, el cual tendras que poner cada vez que accedas al servidor.
* **SHH**: Tienes que crear tu llave publica y privada para el usuario ROOT en el dipositivo donde te vas a loguear. Sube la llave publica al digital ocean ( Este es el comando para generar las llaves SSH: ```ssh-keygen -t rsa -b 4096 -C "tu_correo@ejemplo.com"```).
* > **NOTA**: Dependiendo el metodo que selecciones para autenticar al usuario ROOT ese mismo se va a compartir con todos los nuevos usuarios que agreges a lo largo del tiempo. (En el punto 4.1 viene como autenticar usuario por medio de ssh de forma mas facil)


## **2. Acceder al servidor**
> Asegurate de tener instalado el paquete ssh en el equipo donde vas a conectarte
```bash
# Acceder por contraseña (Cuando le des enter te va a perdir la contraseña)
ssh root@127.0.0.1
```
```bash
# Acceder por ssh
ssh root@127.0.0.1 -i c:/user/.ssh/private_key_ssh
```
* La opción **-i** indica la ubicacion de la llave privada para que pueda acceder al servidor ( Recuerda que la llave publica se subio a digital ocean )


## **3. Actualizar paquetes del sistema**
Cuando accedes por primera vez al VPS tienes que actualizar los paquetes del sistema con los siguientes comando.
```bash
sudo apt update && sudo apt upgrade -y
```
* **apt update** actualiza la lista de paquetes disponibles en los repositorios configurados.
* **apt upgrade** instala las actualizaciones de los paquetes que ya están instalados en el sistema.
* La opción **-y** en **upgrade** responde automáticamente "yes" a cualquier solicitud de confirmación, para que el proceso sea más rápido.


## **4. Crear un usuario con permisos SUDO**
Por seguridad y buenas practicas se recomienda crear un nuevo usuario que pertenezca al grupo de sudo.
Con la finalidad de tener a este super usuario y poder deshabilitar el usuario ROOT mas adelante para agregar una capa de seguridad extra. Todavia no deshabilites el usuario root, hasta que tengas otro usuario con los mismos privilegios y puedas acceder al VPS sin problema

- Accede al servidor con el usuario root y crea un nuevo usuario que pertenezca al grupo SUDO. Al momento de crear un nuevo usuario te pide una contraseña y te crea en la carpeta con el mismo nombre del usuario nuevo en  **home**. Ejemplo ``/home/uAngel``
```bash
adduser uAngel
```
primero tienes que crear el usuario para asignarle una contraseña y luego poder agregarlo a los permisos del sudo

```bash
adduser uAngel sudo
```
* La directiva **sudo** que se agrega al final, es para indicar que ese usuario pertenece al grupo sudo, que le otorga los mismos poderes que el usuario *ROOT*


## **4.1 Acceder con nuevos usuarios por medio de llaves SSH**
Si, seleccionaste el metodo de ssh al momento de crear el servidor. Estas son las instrucciones para iniciar sesion en el VPS por medio de ssh de forma mas eficiente

1. Generar las llaves ssh en tu maquina/compu/cliente/loquesa. Con el siguiente comando:
```bash
ssh-keygen -t rsa -b 4096 -C "tu_correo@ejemplo.
```
2. Accede al servidor con el usuario root.

3. Navega al directorio **/home** y busca la carpeta asignada al nuevo socio que se acaba de crear 
```bash
cd /home
ls 
cd /uAngel
```

4. Verifica si existe la carpeta .ssh y si no la crear, una vez creada necesitamos crear un archivo con el nombre de **authorized_keys** 
```bash
nano authorized_keys
```

5. Regresa a tu equipo local y abre el archivo .pub y copea el contenido, para que lo pegues en el archivo **authorized_keys** que 
acabos de crear y guarda ese archivo
```bash
ctrl + o  
enter  
ctrl + x
```

6. No cierres la sesion del usuario root y intenta acceder en otra consolo el nuevo usuario
```bash
# Acceder por ssh
ssh uAngel@127.0.0.1 -i c:/user/.ssh/private_key_ssh
```
* La opción **-i** indica la ubicacion de la llave privada para que pueda acceder al servidor ( Recuerda que la llave publica se subio a digital ocean )


7. Si el paso 6 funciono correctamente, entonces vamos a facilitar la autenticacion. Regresa a tu maquina local y en la carpeta .ssh
crea un archivo que se llame config (sin ninguna extension) y pega estas instrucciones.
```bash
ServerAliveInterval 120   # Intervalo para verificar la conexión (en segundos)
ServerAliveCountMax 3    # Número de intentos antes de cerrar la conexión

Host my_server_uAngel
    HostName 127.0.0.1
    User uAngel
    IdentityFile c:/user/.ssh/private_key_ssh_uAngel
```
- **Host**: El alias que deseas usar para el servidor (en este ejemplo, mi_servidor).
- **HostName**: La dirección IP o el dominio del servidor.
- **User**: El nombre de usuario con el que deseas conectarte.
- **IdentityFile**: La ruta a tu clave privada (normalmente ~/.ssh/id_rsa).

8. Por ultimo, en lugar de poner toda la linea de conexion solo tenemos que indicar el Host ejemplo
```bash
# Antes 
ssh uAngel@127.0.0.1 -i c:/user/.ssh/private_key_ssh

# Ahora
ssh my_server_uAngel
```

## **5. Deshabilitar el usuario ROOT**
Para mayor seguridad hay que deshabilitar el login con el usuario root. PERO es importante que previamente exista otro usuario que tenga los
mismos privilegios que el ROOT ( como se muestra en el punto 4.1 )

1.  Inicia sesion con un usurio que tenga permisos root y nagecha servicio de ssh
```bash
cd /etc/shh
```

2. Abre el archivo **sshd_config** 
```bash
sudo nano sshd_config
```

3. Busca la directiva **PermitRootLogin**
```bash
PermitRootLogin on  #on significa que si puedes inciar sesion con el root
```

3. Deshabilita el poder hacer login con el root
```bash
PermitRootLogin off 
```

4. Guardar el archivo y recarga el servicio de ssh
```bash
sudo systemctl reload ssh.service
```

5. Verifica si puedes hacer login con el usuario ROOT

> NOTA: Solo deshabilitamos el login del usuario root, pero sigue existiendo si necistas hacer algo son el usuario root solo escribe en la consola ``` sudo su ``` y con eso vas a poder usar al usuario root



## **6. UFW**
Es Ufw sirve para trafico que trafico que entra a nuestro servidor
**NOTA**: Antes de actividad el ufw, ya que por defecto esta desabilidato tenemos que, permitir las conexiones SSH para que de esta forma no te saque de la instacia actual y pierdas el acceso a tu servidor :C 


1. Verificar que uft esta inactivado
```bash
sudo ufw status
```

2. Ver si en la lista de aplicaciones esta OpenSSH
```bash
sudo ufw app list
```

3. Tenemos que permitir conexiones por medio de ssh
```bash
sudo ufw allow "OpenSSH"
```
> **NOTA**: En punto numero **3** es super importante ya que si no permitimos las conexiones por medio de ssh, nos va a cerrar la instacia actual y vamos a perder el acceso al servidor


4. Activar el UFW
```bash
sudo ufw enable
```


## **7. Fail2Ban**
Bloque IP que intentan entrar a la fuerza


1. Instalación
```bash
sudo apt update && sudo apt install fail2ban
```

2. Configurar fail2ban, navega al la caperta donde se instalo fail2ban
```bash
cd /etc/fail2ban
```

3. Tenemos que crear nuestro propio archivo de configuración
```bash
sudo nano jail.local
```
* Se le pone el nombre de jail. por convencion
* Y el .local es para que lo tome como archivo principal de configuracion

4. Ingresa este contenido el archivo
```bash
[DEFAULT]
bantime = 3h #
maxretry = 3


[sshd]
enabled = true # Monitorea las conexiones ssh y bloque el acceso a todas a quellas se pasen el limite de intento pemritido
```

5. Recargar el servicio de fail2ban para que tome las nuevas configuraciones
```bash
# Verifica el status primero
sudo fail2ban-client status 

# Recarga el servicio de fail2ban 
sudo systemctl restart fail2ban.service
```