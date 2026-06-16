## APACHE 
### Instalar un SSL con 4 archivos generados por una autoridad certificadora en debian 13
```
Copiat los cetificados a una carpeta en el servidor, en este caso esta en /home 
root@SRV-TEST:/home/uti# ls
__senado_gob_bo.ca-bundle  __senado_gob_bo.crt	__senado_gob_bo.key  __senado_gob_bo.p7b 

Cambiar el propietario de uti a root

root@SRV-TEST:/home/uti# chown root:root __senado_gob_bo.*
root@SRV-TEST:/home/uti# ls -lh
total 13M
-rw-rw-r-- 1 root root 6.6K Jun 16 11:57 __senado_gob_bo.ca-bundle
-rw-rw-r-- 1 root root 2.3K Jun 16 11:57 __senado_gob_bo.crt
-rw-rw-r-- 1 root root 1.7K Jun 16 11:57 __senado_gob_bo.key
-rw-rw-r-- 1 root root 8.6K Jun 16 11:57 __senado_gob_bo.p7b

Habilitar el modulo con a2enmod y a2enmod headersssl en apache

root@SRV-TEST:/etc/ssl/senado# a2enmod ssl
Considering dependency mime for ssl:
Module mime already enabled
Considering dependency socache_shmcb for ssl:
Module socache_shmcb already enabled
Enabling module ssl.
See /usr/share/doc/apache2/README.Debian.gz on how to configure SSL and create self-signed certificates.
To activate the new configuration, you need to run:
  systemctl restart apache2

root@SRV-TEST:/etc/ssl/senado# a2enmod headers
Enabling module headers.
To activate the new configuration, you need to run:
  systemctl restart apache2
root@SRV-TEST:/etc/ssl/senado# systemctl restart apache2

Crear el archivo de configuracion srv-t.senado.gob.bo-ssl.conf
root@SRV-TEST:/etc/ssl/senado# vim /etc/apache2/sites-available/srv-t.senado.gob.bo-ssl.conf

<VirtualHost *:443>
    ServerName srv-t.senado.gob.bo
    DocumentRoot /var/www/html    
    # === CONFIGURACIÓN SSL ===
    SSLEngine on    
    # Certificado del servidor
    SSLCertificateFile /etc/ssl/senado/__senado_gob_bo.crt    
    # Clave privada
    SSLCertificateKeyFile /etc/ssl/senado/__senado_gob_bo.key    
    # Cadena de certificados intermedios (CA Bundle)
    SSLCertificateChainFile /etc/ssl/senado/__senado_gob_bo.ca-bundle    
    # === CONFIGURACIÓN DE SEGURIDAD (opcional pero recomendada) ===
    # Protocolos seguros
    SSLProtocol all -SSLv3 -TLSv1 -TLSv1.1    
    # Cifrados seguros
    SSLCipherSuite HIGH:!aNULL:!MD5    
    # Headers de seguridad
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-Frame-Options "SAMEORIGIN"
    # === LOGS ===
    ErrorLog ${APACHE_LOG_DIR}/srv-t.senado.gob.bo-ssl-error.log
    CustomLog ${APACHE_LOG_DIR}/srv-t.senado.gob.bo-ssl-access.log combined
</VirtualHost>

Configurar el archivo por defecto para la redireccion de http a https

root@SRV-TEST:/etc/ssl/senado# vim /etc/apache2/sites-available/000-default.conf 
Colocar al final del archivo 
RewriteEngine on
RewriteCond %{SERVER_NAME} =srv-t.senado.gob.bo
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]

Habilitar la configuracion 

root@SRV-TEST:/etc/ssl/senado# a2ensite srv-t.senado.gob.bo-ssl.conf
Enabling site srv-t.senado.gob.bo-ssl.
To activate the new configuration, you need to run:
  systemctl reload apache2
root@SRV-TEST:/etc/ssl/senado# systemctl reload apache2
root@SRV-TEST:/etc/ssl/senado# apache2ctl configtest
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 10.0.0.23. Set the 'ServerName' directive globally to suppress this message
Syntax OK
root@SRV-TEST:/etc/ssl/senado# systemctl restart apache2

Verificar el funcionamiento 

root@SRV-TEST:/etc/ssl/senado# echo | openssl s_client -connect localhost:443 -servername srv-t.senado.gob.bo 2>/dev/null | grep -E "subject=|issuer="
subject=CN=*.senado.gob.bo
issuer=C=GB, O=Sectigo Limited, CN=Sectigo Public Server Authentication CA DV R36
root@SRV-TEST:/etc/ssl/senado# curl -I https://srv-t.senado.gob.bo
HTTP/1.1 200 OK
Date: Tue, 16 Jun 2026 16:08:50 GMT
Server: Apache/2.4.67 (Debian)
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Last-Modified: Tue, 02 Jun 2026 21:33:37 GMT
ETag: "29cf-6534c10611532"
Accept-Ranges: bytes
Content-Length: 10703
Vary: Accept-Encoding
Content-Type: text/html
```

### Corregir erros simples
```
Editar el archivo de configuracion de apache
vim /etc/apache2/apache2.conf
Agregar al final del archivo 
  ServerName srv-t.senado.gob.bo
Reinicar servicios 
systemctl restart apache2
systemctl status apache2
apache2ctl configtest
```
