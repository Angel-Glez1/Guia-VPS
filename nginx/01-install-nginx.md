# NGINX - VPS

1. Antes de instalar nginx, hay que actualizar los paquetes de nuestro servidor
```bash
sudo apt update
```

2. Instalar Nginx.
```bash
sudo apt install nginx
```

3. Habilitar el puerto 80 y 443 con ufw.
```bash
sudo ufw allow "Nginx Full"
```
* **Puerto 80**: Es el puerto estándar para el tráfico HTTP 
* **Puerto 443**: Es el puerto estándar para el tráfico HTTPS


4. Validar si la habilitación de los puertos 80 y 443 fue correcta
```bash
sudo ufw status verbose

# El anterior comando, tiene que retornar esto. En caso de que no revisar porque no se activaron los puertos
Status: active
To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx Full                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx Full (v6)            ALLOW       Anywhere (v6)
```


5. Por ultimo elimina el enlace simbolico del proyecto **default** que viene con ngix. Con la finalidad de deshabilitar el sitio PERO sin perder la configuracion del mismo. Para que si lo queremos activar otra vez unicamente volver a crear el link simbolico
```bash
# Elimina el enlace simbolico, pero mantiene el archivo original ubicado en sites-available
sudo rm /etc/nginx/sites-enabled/default 

# Reinica el servicio de nginx
sudo systemctl restart nginx.service
```