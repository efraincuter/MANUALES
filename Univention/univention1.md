# Instalacion de Univention 

### Preguntat por el DNS

nslookup senado.gob.bo
```
Crear el SubDomion en Domain > DNS, seleccionar el nombre, en este ejemplo senado.gob.bo
    # Adicionar ADD > DNS HOST RECORD
        - Hostname : test
        - IP Addresses : 10.0.0.199

    #Ingresar a la IP en la misma opcion anterior > ADD > DNS : Point Record
        -IP Address : 10.0.0.199
        -Pointer record : test.senado.gob.bo

DNS: Host record A (y AAAA) Asocia un nombre de equipo a una dirección IP 
DNS: Alias record  CNAME  Crea un alias o apodo para un nombre de equipo que ya existe 
DNS: Service record SRV Define la ubicación (dirección y puerto) de un servicio específico en la red, como un servidor LDAP o Kerberos
DNS: Pointer record PTR Hace la resolución inversa: dada una dirección IP, devuelve un nombre de equipo

En el caso de tener que apuntar senado.gob.bo y www.senado.gob.bo a la IP 10.0.0.119

Crear un registro senado.gob.bo. y apuntar al 10.0.0.119
Crear un registro www y apuntar al 10.0.0.119

```

### Para verificar en el equipo windows se debe hacer con estos comandos en powershell
```
wmic computersystem get domain
net config workstation
```


