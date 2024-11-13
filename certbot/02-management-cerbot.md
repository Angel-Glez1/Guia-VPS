# Comandos utils para manejar certbot


## 1. Renovar certificados manualmente
```bash
sudo certbot renew
```

## 2. Ver la lista de certificados instalados
```bash
sudo certbot certificates
```

## 3. Eliminar un certificado específico
```bash
sudo certbot delete --cert-name tu-dominio.com
```

## 5. Renovar y recargar automáticamente Nginx
```bash
sudo certbot renew --deploy-hook "systemctl reload nginx"
```





