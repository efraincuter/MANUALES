# Distribucion > Debian 13

### Crear el repositorio en la ruta /etc/apt/sources.list.d/ el archivo debian.sources 

```
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

### Agregar al PATH en root vim /root/.bashrc, adicionarlo al final
```
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

### Actualizar un paquete,descargar el paquete e instalarlo
```
sudo dpkg -i sshpilot_5.4.5-1_all.deb
sudo apt install -f (en caso de resolver dependencias)
```