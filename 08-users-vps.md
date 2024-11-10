# Usuarios

Para poder administrar correctamente un servidor, necesitamos crear usuarios y asignarles los permisos necesarios.
Para crear un nuevo usuario, es necesario estas autenticados con un usurio que pertenezca al grupo **SUDO** o en su defecto el usuario **ROOT** si iniciaste el servidor por primera vez


Comando para crear un usuario normal
```bash
adduser new_user
```

Para crear un usuario con permisos root, se tiene que agregar al grupo sudo
```bash
adduser new_user sudo
```
> nota **En ambos cosas te van a solicitar una contraseña para el nuevo usuario**


## Autenticación con contraseña

Es muy sencillo corre el comando para conectarte al VPS, pero ahora usando el nuevo usuario, y pon la contraseña que esta
asociada al nuevo usuario
```bash
ssh new_user@127.0.0.1
```

## Autenticación con SSH

Si queremos acceder al servidor con el nuevo usuario por medio de SSH, tenemos que seguir estos pasos.

1. Crear la carpeta .ssh y dentro de esa carpeta el archivo authorized_key
```bash
cd /home/new_user
mkdir .ssh
nano authorized_keys 
```

2. Ahora en el dispositivo local donde vas a acceder al servidor genera las llaves ssh, con el siguiente comando
```bash
ssh-keygen -t rsa -b 4096 -C "tu_correo@ejemplo.com"
```

3. Ahora copia el contenido de la llave publica y pega el contenido en el archivo authorized_keys del nuevo usuario
```bash
nano authorized_keys
>> rsa 12414234
```

4. Agrega en el archivo config los datos del nuevo socio para que puedas acceder al servidor de forma mas facil
Host name_conexion
    HostName 127.160.1.1
    User new_user
    IdentityFile C:\Users\alcar\.ssh\new_user