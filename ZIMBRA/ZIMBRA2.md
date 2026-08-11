# ZIMBRA INSTALACION DE CERTIFICADO
## Revision del sistema operativo
```
Ubicacion
/opt/zimbra/ssl/zimbra/commercial

[root@correo commercial]# ls
commercial_ca.crt  commercial.crt  commercial.key
```
## REVISAR EL CERTIFICADO Y LA CLAVE PRIVADA
```
[root@correo commercial]# openssl x509 -in commercial.crt -subject -dates -noout
subject= /CN=correo.senado.gob.bo
notBefore=Jun  3 18:44:12 2026 GMT
notAfter=Sep  1 18:44:11 2026 GMT

root@correo commercial]# openssl rsa -in commercial.key -check -noout
RSA key ok

REVISAR QUE CLAVE Y CERTIFICADO COINCIDAN
root@correo commercial]# openssl rsa -in commercial.key -modulus -noout | md5sum
fbef5405ebd8020acdf0dec80983f31b  -
[root@correo commercial]# openssl x509 -in commercial.crt -modulus -noout | md5sum
fbef5405ebd8020acdf0dec80983f31b  -

VERIFICAR QUE TODO FUNCIONE JUNTO
[root@correo commercial]# openssl verify -CAfile commercial_ca.crt -verbose commercial.crt
commercial.crt: OK

Revision de los permisos 
[root@correo commercial]# ls -la /opt/zimbra/ssl/zimbra/commercial/
total 28
drwxr-x--- 2 zimbra zimbra 4096 Jul  5  2021 .
drwxr-x--- 5 zimbra zimbra 4096 Jun  7  2021 ..
-rw-r----- 1 zimbra zimbra 5810 Jun  3 15:50 commercial_ca.crt
-rw-r----- 1 zimbra zimbra 7957 Jun  3 15:50 commercial.crt
-rw-r----- 1 zimbra zimbra 3272 Jun  3 15:50 commercial.key

```
## Revisar el estado del zimbra
```
[root@correo ~]# su zimbra
[zimbra@correo root]$ zmcontrol status
Host correo.senado.gob.bo
	amavis                  Running
	antispam                Running
	antivirus               Running
	ldap                    Running
	logger                  Running
	mailbox                 Running
	memcached               Running
	mta                     Running
	opendkim                Running
	proxy                   Running
	service webapp          Running
	snmp                    Running
	spell                   Running
	stats                   Running
	zimbra webapp           Running
	zimbraAdmin webapp      Running
	zimlet webapp           Running
	zmconfigd               Running


Este comando te dirá si el servicio proxy (Nginx) está funcionando correctamente
[zimbra@correo root]$ zmproxyctl status
proxy is running.

[zimbra@correo root]$ zmproxyctl status
proxy is running.
[zimbra@correo root]$ pwd

[zimbra@correo root]$ cd
[zimbra@correo ~]$ zmcertmgr viewdeployedcrt all
[root@correo ~]# sudo -u zimbra /opt/zimbra/bin/zmcertmgr viewdeployedcrt all

- imapd: /opt/zimbra/conf/imapd.crt
notBefore=Jun  3 18:44:12 2026 GMT
notAfter=Sep  1 18:44:11 2026 GMT
subject=CN = correo.senado.gob.bo
issuer=C = US, O = Let's Encrypt, CN = YR2
SubjectAltName=correo.senado.gob.bo
- ldap: /opt/zimbra/conf/slapd.crt
notBefore=Jun  3 18:44:12 2026 GMT
notAfter=Sep  1 18:44:11 2026 GMT
subject=CN = correo.senado.gob.bo
issuer=C = US, O = Let's Encrypt, CN = YR2
SubjectAltName=correo.senado.gob.bo
- mailboxd: /opt/zimbra/mailboxd/etc/mailboxd.pem
notBefore=Jun  3 18:44:12 2026 GMT
notAfter=Sep  1 18:44:11 2026 GMT
subject=CN = correo.senado.gob.bo
issuer=C = US, O = Let's Encrypt, CN = YR2
SubjectAltName=correo.senado.gob.bo
- mta: /opt/zimbra/conf/smtpd.crt
notBefore=Jun  3 18:44:12 2026 GMT
notAfter=Sep  1 18:44:11 2026 GMT
subject=CN = correo.senado.gob.bo
issuer=C = US, O = Let's Encrypt, CN = YR2
SubjectAltName=correo.senado.gob.bo
- proxy: /opt/zimbra/conf/nginx.crt
notBefore=Jun  3 18:44:12 2026 GMT
notAfter=Sep  1 18:44:11 2026 GMT
subject=CN = correo.senado.gob.bo
issuer=C = US, O = Let's Encrypt, CN = YR2
SubjectAltName=correo.senado.gob.bo

Ubicacion de los archivos 
[zimbra@correo root]$ ls -la /opt/zimbra/ssl/zimbra/commercial/
total 28
drwxr-x--- 2 zimbra zimbra 4096 Jul  5  2021 .
drwxr-x--- 5 zimbra zimbra 4096 Jun  7  2021 ..
-rw-r----- 1 zimbra zimbra 5810 Jun  3 15:50 commercial_ca.crt
-rw-r----- 1 zimbra zimbra 7957 Jun  3 15:50 commercial.crt
-rw-r----- 1 zimbra zimbra 3272 Jun  3 15:50 commercial.key

```