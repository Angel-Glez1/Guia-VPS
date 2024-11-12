# Gestión de archivos (crear, eliminar, mover, mostrar)

1. Crear ó Editar 
```bash
touch ./root/ids.txt # Solo creo el archivo
nano ./root/ids.txt # Crea el archivo y abre un editor de texto (nano)
```
> Para guardar archivo y salir de nano
```bash
ctrl + o && enter && ctrl + x
``` 

2. Mostar el contenido del archivo
```bash
cat ids.txt
```


3. Mover archivo
```bash
cp -r <nombre_archivo> <destino>

# Ejemplo
mv ids.txt ./docs/ # Mover con el mismo nombre
mv ids.txt ./docs/new.ids.txt # Mover y cambiar el nombre
```


4. Copear archivo
```bash
cp -r <nombre_archivo> <destino>

# Ejemplo
cp ids.txt ./docs/copy.ids.text # cambiar el nombre
cp ids.txt ./docs/ # dejar el mismo nombre
```

5. Eliminar
```bash
rm ids.txt
```

# Gestión de carpetas (crear, eliminar, mover, mostrar)
1. Crear
```bash
mkdor projects
```
 
2. Mostar el contenido la carpeta
```bash
cd projects
ls
```

3. Mover ó cambiar el nombre
```bash
mv <nombre_carpeta> <destino>
# Ejemplo
mv projects ../docs/ # mover con el mismos nombre 
mv projects ../docs/old_projects # mover y cambiar el nombre 
```

4. Copear archivo
```bash
cp -r <nombre_carpte> <destino>

# Ejemplo
cp -r ./projects ./docs/projects_backup 
```

4. Eliminar
```bash
rm -r ./projects 
```



