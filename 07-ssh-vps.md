# Configurar SSH en un VPS


Al igual que en un ambiente local (computadora, laptop, etc), se puede configurar y crear 
llaves ssh para que nuestro sistema se pueda autenticar a otros servidores o servicios que ocupen esto.
Ejemplo: github 


Para hacer esta configuracion hay que correr los siguientes comandos.

1. Generar las llaves ssh
```bash
ssh-keygen -t rsa -b 4096 -C "tu_correo@ejemplo.com"
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

4. 