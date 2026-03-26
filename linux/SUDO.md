# SUDO 
## En debian 13.3 (trixie)
### Buscar el paquete
```
apt-cache search sudo
```
### Instalar SUDO 
```
apt-get install sudo
```
### Agrega un usuario al grupo sudo 
```
usermod -aG sudo USUARIO
```
### Verificar que pertenece al grupo de SUDO
```
id usuario
```
### Verificar directamente en el grupo, y salir el usuario de su sesion 
```
getent group sudo
```

## En Ubuntu 24 
```
Adcionar un usuario adduser desarrollo
Colocar en el grupo SUDO usermod -aG sudo desarrollo
```
