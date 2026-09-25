# ZIMBRA INSTALACION DE CERTIFICADO  Wildcaard
## Revisar si se tiene una licencia comercial gratuita
```
[root@correo ~]# ls /opt/zimbra/ssl/zimbra/commercial/
commercial_ca.crt  commercial.crt  commercial.key
```
## Copiar los certificados wldacard de senado.gobo y remplazar los gratuitos
```
root@correo ~]# cp /tmp/certificados/__senado_gob_bo.crt /opt/zimbra/ssl/zimbra/commercial/commercial.crt
cp: overwrite ‘/opt/zimbra/ssl/zimbra/commercial/commercial.crt’? y
[root@correo ~]# cp /tmp/certificados/__senado_gob_bo.key /opt/zimbra/ssl/zimbra/commercial/commercial.key
cp: overwrite ‘/opt/zimbra/ssl/zimbra/commercial/commercial.key’? y
[root@correo ~]# cp /tmp/certificados/__senado_gob_bo.ca-bundle /opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt
cp: overwrite ‘/opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt’? y

coocar los permisos 
[root@correo ~]# chown zimbra:zimbra /opt/zimbra/ssl/zimbra/commercial/*
[root@correo ~]# chmod 640 /opt/zimbra/ssl/zimbra/commercial/commercial.key
```
## verificar y Desplegar el certificado 
```
[root@correo ~]# su - zimbra
Last login: Tue Sep  1 16:27:49 -04 2026 on pts/0
[zimbra@correo ~]$ /opt/zimbra/bin/zmcertmgr verifycrt comm \
>   /opt/zimbra/ssl/zimbra/commercial/commercial.key \
>   /opt/zimbra/ssl/zimbra/commercial/commercial.crt \
>   /opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt

[zimbra@correo ~]$ /opt/zimbra/bin/zmcertmgr deploycrt comm \
>   /opt/zimbra/ssl/zimbra/commercial/commercial.crt \
>   /opt/zimbra/ssl/zimbra/commercial/commercial_ca.crt

```
## revisar y renicar los servicios 
```
[zimbra@correo ~]$ zmcertmgr viewdeployedcrt
[zimbra@correo ~]$ zmcontrol stop
[zimbra@correo ~]$ zmcontrol start
[zimbra@correo ~]$ zmcontrol status
```