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