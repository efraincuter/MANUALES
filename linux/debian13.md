# Distribucion > Debian 13

## Crear repositorio
```
En la ruta /etc/apt/sources.list.d/ , crear el archivo debian.sources 

Types: deb
URIs: https://deb.debian.org/debian
Suites: trixie trixie-updates
Components: main non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg


Types: deb
URIs: https://security.debian.org/debian-security
Suites: trixie-security
Components: main non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```

## 
```
Revisar el paquete SUDO, con  apt-cache search sudo 
Instalar apt install sudo
Crear un usuario adduser desarrollo
Colocarlo como sudo usermod -aG sudo desarrollo
