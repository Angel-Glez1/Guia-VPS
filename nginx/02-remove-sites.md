# Remover un sitio de internet

1.  Ve a tu proveedor de DNS y elimina el dominio/subnodmio que ya no quieres que este habilitado

2. Eliminar del servidor el proyecto del sitio, osea si es un proyecto de react elimina sus carpeta y todo eso.

3. Eliminar los enlaces simbolicos de NGIX
```bash
sudo rm -r /etc/ngix/sites-enabled/tu-sitio.com
```

4. Eliminar los virtual host (Opcional) 
```bash
sudo rm -r /etc/ngix/sites-available/tu-sitio.com
```

5. Elimiar los certificados SSL 
```bash
sudo certbot revoke --cert-name=<dominio.com>
```

6. Recarga el servicio de nginx
```bash
sudo systemctl reload nginx.service
```


