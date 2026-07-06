## Instalacion de Lets Encrypt en nginx

### Instalar Certbot y su Plugin para Nginx
```
sudo apt install certbot python3-certbot-nginx -y
```
### configurar el cerbot con el dominio srv-t.senado.bo
```
uti@srv-test-nginx:~$ sudo certbot --nginx -d srv-t.senado.bo

Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): efrain.cuter@senado.gob.bo

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.7-June-04-2026.pdf. You must agree in
order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y
Account registered.
Requesting a certificate for srv-t.senado.bo

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/srv-t.senado.bo/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/srv-t.senado.bo/privkey.pem
This certificate expires on 2026-09-30.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for srv-t.senado.bo to /etc/nginx/sites-enabled/default
Congratulations! You have successfully enabled HTTPS on https://srv-t.senado.bo

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

uti@srv-test-nginx:~$ curl -I https://srv-t.senado.bo
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Thu, 02 Jul 2026 18:19:32 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Fri, 26 Jun 2026 15:10:27 GMT
Connection: keep-alive
ETag: "6a3e9663-267"
Accept-Ranges: bytes

```

### El cambio del archivo default es en la ruta /etc/nginx/sites-available
```
Ese es un ejemplo de cambio del artchivo una vez ejecutado el certbot
  listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/srv-t.senado.bo/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/srv-t.senado.bo/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
server {
    if ($host = srv-t.senado.bo) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


	listen 80 ;
	listen [::]:80 ;
    server_name srv-t.senado.bo;
    return 404; # managed by Certbot



fullchain.pem	El certificado público + la cadena de confianza
privkey.pem	La clave privada (¡Mantener segura!)
cert.pem	Solo el certificado público
chain.pem	Solo la cadena de certificados intermedios

```
### Elminar el lets encrypt 
```
uti@srv-test-nginx:~$ sudo certbot certificates
[sudo] password for uti: 
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Found the following certs:
  Certificate Name: srv-t.senado.bo
    Serial Number: 5b67cae48892bf0b54476d430a165c32b99
    Key Type: ECDSA
    Domains: srv-t.senado.bo
    Expiry Date: 2026-09-30 17:19:44+00:00 (VALID: 89 days)
    Certificate Path: /etc/letsencrypt/live/srv-t.senado.bo/fullchain.pem
    Private Key Path: /etc/letsencrypt/live/srv-t.senado.bo/privkey.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
uti@srv-test-nginx:~$ sudo certbot delete --cert-name srv-t.senado.bo
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
The following certificate(s) are selected for deletion:

  * srv-t.senado.bo

WARNING: Before continuing, ensure that the listed certificates are not being
used by any installed server software (e.g. Apache, nginx, mail servers).
Deleting a certificate that is still being used will cause the server software
to stop working. See https://certbot.org/deleting-certs for information on
deleting certificates safely.

Are you sure you want to delete the above certificate(s)?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y
Deleted all files relating to certificate srv-t.senado.bo.

cd /etc/nginx/sites-available/

sudo cp default default.original
sudo vim default   (el archivo por defecto deberia quedar asi una vez elminado el SSL)

server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/html;

	# Add index.php to the list if you are using PHP
	index index.html index.htm index.nginx-debian.html;

	server_name _;

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}
}

sudo nginx -t
sudo systemctl reload nginx
ls /etc/letsencrypt/
sudo rm -rt /etc/letsencrypt/
sudo rm -rf /etc/letsencrypt/
sudo rm -rf /var/log/letsencrypt/
sudo rm -rf /var/lib/letsencrypt/

uti@srv-test-nginx:~$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Fri, 03 Jul 2026 17:53:38 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Fri, 26 Jun 2026 15:10:27 GMT
Connection: keep-alive
ETag: "6a3e9663-267"
Accept-Ranges: bytes
```

### Instalar certifcado comerial wildcard *.senado.bo
```
    __senado_bo.crt - Certificado público del servidor
    __senado_bo.key - Clave privada
    __senado_bo.ca-bundle - Cadena de certificados intermedios
    __senado_bo.p7b - Contenedor PKCS#7 (opcional pero útil)

Crear una carpeta donde se colocaran los certificados
cd /etc/ssl/
sudo mkdir senado.bo
cd /etc/ssl/senado.bo/

Cambiar los permisos 
sudo chmod 600 __senado_bo.key 
sudo chmod 644 __senado_bo.crt 
sudo chmod 644 __senado_bo.ca-bundle 
sudo chown root:root *.*

Editar el archivo de configuracion y deber ser parecido al que se uso en la configuracion 
sudo vim /etc/nginx/sites-available/default

# Bloque HTTP
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	server_name srv-t.senado.bo;
	return 301 https://$server_name$request_uri;
}

# Bloque HTTPS 
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name srv-t.senado.bo;

    # Certificados SSL
    ssl_certificate /etc/ssl/senado.bo/fullchain.pem;
    ssl_certificate_key /etc/ssl/senado.bo/__senado_bo.key;

    # Configuración SSL recomendada
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    # Configuración de la página web
    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

Se debe crear un archivo fullchain.pem por que nginx necesita el certificado los intermedios juntos en un archivo 
sudo openssl pkcs7 -in /etc/ssl/senado.bo/__senado_bo.p7b -print_certs -out /etc/ssl/senado.bo/fullchain.pem

sudo nginx -t
  nginx: the configuration file /etc/nginx/nginx.conf syntax is ok

sudo systemctl reload nginx

curl -I https://srv-t.senado.bo
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 06 Jul 2026 16:13:37 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Fri, 26 Jun 2026 15:10:27 GMT
Connection: keep-alive
ETag: "6a3e9663-267"
Accept-Ranges: bytes

```
