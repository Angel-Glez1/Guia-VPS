
# Configuración de Certificado SSL en Ubuntu con Nginx

Esta guía te ayudará a configurar un certificado SSL en tu servidor Ubuntu con Nginx utilizando Certbot y Let's Encrypt.

## 1. Instalar Certbot y el complemento de Nginx
Certbot es una herramienta gratuita que automatiza la instalación de certificados SSL de Let’s Encrypt.

Ejecuta los siguientes comandos para instalar Certbot y el complemento de Nginx:
```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
```

## 2. Obtener un certificado SSL
Ejecuta Certbot con el complemento de Nginx para obtener el certificado SSL:
```bash
sudo certbot --nginx -d angel-alcaraz.com -d www.angel-alcaraz.com
```
- **`-d`** especifica el dominio para el cual estás solicitando el certificado.
- Certbot automáticamente edita la configuración de Nginx para usar SSL y recarga el servidor.

## 3. Verificar la renovación automática
Certbot configura automáticamente la renovación del certificado. Puedes verificar la renovación automática con:
```bash
sudo systemctl status certbot.timer
```
Para probar la renovación manualmente, ejecuta:
```bash
sudo certbot renew --dry-run
```

## 4. Configurar manualmente Nginx (opcional)
Si necesitas configurar manualmente Nginx, edita tu archivo de configuración en `/etc/nginx/sites-available/tu-dominio`:
```nginx
server {
    listen 80;
    server_name tu-dominio.com www.tu-dominio.com;

    # Redirigir todas las solicitudes HTTP a HTTPS
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name tu-dominio.com www.tu-dominio.com;

    ssl_certificate /etc/letsencrypt/live/tu-dominio.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/tu-dominio.com/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384";
    ssl_prefer_server_ciphers on;

    location / {
        # Tu configuración de proxy o de root
        proxy_pass http://localhost:3000;  # Por ejemplo, para una aplicación en Node.js
    }
}
```
Guarda los cambios y reinicia Nginx:
```bash
sudo nginx -t  # Para verificar la configuración
sudo systemctl restart nginx
```

## 5. Habilitar la renovación aubtomática (si no se hizo automáticamente)
Añade una tarea cron manual si es necesario:
```bash
echo "0 0,12 * * * root certbot renew --quiet" | sudo tee -a /etc/crontab > /dev/null
```

## Conclusión
Siguiendo estos pasos, tu servidor Ubuntu con Nginx tendrá un certificado SSL activo y se renovará automáticamente. Esto asegura que la conexión sea segura y cifrada para tus visitantes.

