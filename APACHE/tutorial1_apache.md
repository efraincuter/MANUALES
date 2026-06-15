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
### Revisar la clave
```
openssl ec -in /etc/letsencrypt/live/srv-t.senado.gob.bo/privkey.pem -text -noout | head -10
root@SRV-TEST:/etc/letsencrypt/live#  openssl ec -in /etc/letsencrypt/live/srv-t.senado.gob.bo/privkey.pem -text -noout | head -10
read EC key
Private-Key: (256 bit)
priv:
    b1:f2:cd:fc:1b:a0:4d:b4:4d:12:49:09:84:d3:40:
    23:fb:24:3b:00:b5:49:49:11:f8:09:81:55:ea:a2:
    dd:5e
pub:
    04:11:79:ee:22:b0:7c:c5:34:4b:15:8a:04:24:54:
    7b:21:9c:cc:de:1c:cf:74:42:ed:b2:3e:2a:86:df:
    9a:85:8d:a1:70:74:99:da:37:f5:82:e0:63:e3:05:
    28:e4:46:67:cb:ca:7d:ee:20:06:dd:95:7b:2c:68:

openssl pkey -in /etc/letsencrypt/live/srv-t.senado.gob.bo/privkey.pem -text -noout | grep -E "Private-Key|ASN1 OID|NIST CURVE"
```
