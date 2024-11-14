# Configurar SSH en un VPS


Al igual que en un ambiente local (computadora, laptop, etc), se puede configurar y crear 
llaves ssh para que nuestro sistema se pueda autenticar a otros servidores o servicios que ocupen esto.
Ejemplo: github 

> Nota: **Este proceso se tiene que hacer con el usario root, si tienes deshabilitado el login con root, puedes usar el siguiente comando para entrar en modo superusuario `sudo su` y para salir del modo superusuario solo ocupa `exit`)**


1. Generar las llaves ssh
```bash
ssh-keygen -t rsa -b 4096
```

2. Hay que permitir que nuestras llaves ssh se puedan contectar con los sistemas externos
```bash
eval  $(ssh-agent -s)
```
> nota: El comando **ssh-agent** retorna una lista de instrucciones que se tienen que ejecutar mediante la consola, pero con la funcion eval nos podemos saltar ese paso ya que ella se encargara de ejecutar las instrucciones que retorna ssh-agent

3. Agregar la llave privada, que se genero anterior mente.
```bash
ssh-add id_rsa
```

4. Copea el contenido de la llave publica y pegala en el sistema donde te queires autenticar por ssh