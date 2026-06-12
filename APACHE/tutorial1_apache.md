## APACHE 
### Instalr un SSL con cerbot en un servidor debian 13 y apache
```
sudo apt update
sudo apt upgrade -y
sudo apt install certbot python3-certbot-apache
sudo certbot --apache -d srv-t.senado.gob.bo (Llenar los datos que se generan)
```
### Revisar la estrcutura 
```
Certificado /etc/letsencrypt/live/test.cuter.net/fullchain.pem   (certificado SSL publico)
Claver privada /etc/letsencrypt/live/test.cuter.net/privkey.pem   (Clave privada)
Configuracion SSL /etc/apache2/sites-available/test.cuter.net-le-ssl.conf  (Virtual Host https)

cert.pem -> es el certificado del dominio srv-t.senadog.gob.bo
chain.pem -> Cadena de certificados intermedios
fullchain.pem -> cert.pem + chain.pem (el que usa Apache)
privkey.pem -> Tu clave privada
```
### Revisar cert.pem 
```
# openssl x509 -in /etc/letsencrypt/live/srv-t.senado.gob.bo/cert.pem -text -noout   (forma de verificar informacion detallada) 

# openssl x509 -in /etc/letsencrypt/live/srv-t.senado.gob.bo/cert.pem -issuer -subject -dates -noout (informacion mas detallada)

root@SRV-TEST:/etc/letsencrypt/live# openssl x509 -in /etc/letsencrypt/live/srv-t.senado.gob.bo/cert.pem -issuer -subject -dates -noout
issuer=C=US, O=Let's Encrypt, CN=YE1
subject=CN=srv-t.senado.gob.bo
notBefore=Jun 12 19:05:53 2026 GMT
notAfter=Sep 10 19:05:52 2026 GMT
```
### Revisar el archivo que de configuracion de apache para el SSL que es generado con el cerbot
```
cat /etc/apache2/sites-available/000-default-le-ssl.conf | grep -E "SSLCertificate|ServerName" (Ver la configuracion de apache en debian 13)
```
### Reviasr el arcihvo chain.pem
```
Ver cuantos certidicados tiene la cadena
# openssl crl2pkcs7 -nocrl -certfile /etc/letsencrypt/live/srv-t.senado.gob.bo/chain.pem | openssl pkcs7 -print_certs -noout | grep "subject="

Ver el emisor y solo el sujeto de la cadena en este ejemplo para Let's Encrypt 
# openssl crl2pkcs7 -nocrl -certfile /etc/letsencrypt/live/srv-t.senado.gob.bo/chain.pem | openssl pkcs7 -print_certs -noout | grep -E "subject=|issuer="
```

### Revisar el fullchain.pem
```
Ver todos los certificados que estan en el archivo
# openssl crl2pkcs7 -nocrl -certfile /etc/letsencrypt/live/srv-t.senado.gob.bo/fullchain.pem | openssl pkcs7 -print_certs -noout
Ver cuantos certificados tiene 
#  grep -c "BEGIN CERTIFICATE" /etc/letsencrypt/live/srv-t.senado.gob.bo/fullchain.pem
veridicar que apache lo lee 
# openssl x509 -in /etc/letsencrypt/live/srv-t.senado.gob.bo/fullchain.pem -text -noout | head -20

Verificar que la cedena esta completa y es valida
# openssl verify -untrusted /etc/letsencrypt/live/srv-t.senado.gob.bo/chain.pem /etc/letsencrypt/live/srv-t.senado.gob.bo/cert.pem
/etc/letsencrypt/live/srv-t.senado.gob.bo/cert.pem: OK
``` 
### Revisar la cadena
```

```
