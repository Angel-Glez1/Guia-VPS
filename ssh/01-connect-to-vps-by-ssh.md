# Conexion con el VPS por llaves SSH

## 1. Genera las llaves ssh.
Corre el siguiente comando en tu maquina local.
```bash
ssh-keygen -t rsa
```
`Nota`: Al ejecutar el comando, se te pedirá confirmar la ubicación donde se guardarán las llaves y si deseas agregar una contraseña a las llaves.

## 2. Carga la llave publica
El comando genera dos archivos. Abre el archivo con extensión .pub, copia su contenido y súbelo a DigitalOcean.


## 3. Conectarse al VPS por ssh
Para conectarte al VPS por medio de llaves ssh, corre el siguiente comando en tu consola de tu maquina local.
```bash
ssh root@123.12.12.1 -i c:/user/.ssh/private_key_ssh
```
* La opción **-i** indica la ubicación de la clave privada para acceder al servidor (recuerda que la clave pública se subió a Digital Ocean).

## 4. Automatizar la conexion.
En la carpeta `.ssh` en tu maquiena local, crea un archivo llamado `config` (sin extensión) y agrega el siguiente contenido, reemplazando los valores correspondientes:
```bash
ServerAliveInterval 120     # Intervalo para verificar la conexión (en segundos)
ServerAliveCountMax 3      # Número de intentos antes de cerrar la conexión

Host mi_servidor_prod
    HostName ip_del_servidor_o_dominio
    User usuario
    IdentityFile ~/.ssh/id_rsa
    Port 22
```
- **Host**: El alias que deseas usar para el servidor (en este ejemplo, mi_servidor).
- **HostName**: La dirección IP o el dominio del servidor.
- **User**: El nombre de usuario con el que deseas conectarte.
- **IdentityFile**: La ruta a tu clave privada (normalmente ~/.ssh/id_rsa).

## Conectate 
Para conectarse al VPS me diante a la configuracion que hicimos:
```bash
ssh mi_servidor_prod
```





