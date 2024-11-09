# Conexión con el VPS por contraseña
1- Para realizar la conecxion al VPS por medio de Contraseña, ejecuta el siguente comando y despues de va a solicitar la contraseña creada en el proveedor 
```
ssh <user>@<ip_server|dominio> 
```


# Conexion con el VPS por llaves SSH
1- Para este metodo, primero tenemos que crear las llaves ssh, del dispositivo que se va a conectar, para lograr eso hay que correr el siguiente comando
```bash
ssh-keygen -t rsa -b 4096 -C "tu_correo@ejemplo.com"
```
> Notas: al correr el comando te va a pedir que confirmes la ubi, donde va a guardar las llaves y tambien si quieres agregarle un contraseña a esa llaves

1.1- Una vez generadas las llaves **pub** y **priv** copea el contenido del archivo **.pub** y subelo al proveedor. 

2- Para lograr conectarnos al VPS tenemos que correr el siguente comando
```bash
ssh <user>@<ip_server|dominio> -i <path_de_la_llave_privada>
```

## Automatizar la conexion por llaves ssh
1. En la carpeta .ssh crear un archivo que se llame config, sin ningun tipo de extension y
agrega el siguiente contenido y remplaza los valores correspondientes
```bash
ServerAliveInterval 120     # Intervalo para verificar la conexión (en segundos)
ServerAliveCountMax 3      # Número de intentos antes de cerrar la conexión

Host mi_servidor_dev
    HostName ip_del_servidor_o_dominio
    User usuario
    IdentityFile ~/.ssh/id_rsa
    Port 22

Host mi_servidor_prod
    HostName ip_del_servidor_o_dominio
    User usuario
    IdentityFile ~/.ssh/id_rsa
    Port 22
```
- Host: El alias que deseas usar para el servidor (en este ejemplo, mi_servidor).
- HostName: La dirección IP o el dominio del servidor.
- User: El nombre de usuario con el que deseas conectarte.
- IdentityFile: La ruta a tu clave privada (normalmente ~/.ssh/id_rsa).
> Luego, cuando ejecutas:
```bash
ssh mi_servidor_dev
ssh mi_servidor_prod
```




# Salir del VPS
1- Usua los siguiente comandos
```bash
ctrl + d
```bash
1- Usua los siguiente comandos
```
exit
```





