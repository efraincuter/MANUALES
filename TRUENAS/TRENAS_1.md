# TRUENAS 25.04.2.6

### Crear USUARIOS
```
 Credentials > Users
    - UTI1,UTI2
    - RRHH

Crear grupos y adicioanr los usuarios a los grupos
    Credentials > Groups 
        -GUTI : Check > SMB Group   --> GUTI > Members > UTI1,UTI2
        -GRRHH : Check > SMB Group  --> GRRHH > Members > RRHH
```

### Crear el DATASET
```
    - NAME : UTI  > DATASET PRESENT : SMB > UTI > 
        Permissions > Edit ---> Elminar Group-builtin_usres 
        Adicionar Add Item : Who - Group > seleccionar el grupp GUTI > En la parte de Permissions : Full Control

### AUDITAR
```
  Para revisar el tema de logs entrar a System > Audit >> y colcar de la siguinete manera para revisar Service = "SMB" AND User = "efrain.cuter" 
```

### REVISAR LA CONEXION DE WINDOWS 10 y 11 EN POWERSHELL
```
PS C:\Windows\system32> Get-SmbConnection

ServerName ShareName       UserName            Credential           Dialect NumOpens
---------- ---------       --------            ----------           ------- --------
10.0.0.28  archivo-central SENADO\pedro.choque SENADO\susana.aquino 3.1.1   1
10.0.0.28  IPC$            SENADO\pedro.choque SENADO\susana.aquino 3.1.1   1


Para e iniciar el servicio 
PS C:\Windows\system32> Stop-Service -Name "LanmanWorkstation" -Force
PS C:\Windows\system32> Start-Service -Name "LanmanWorkstation"
```
