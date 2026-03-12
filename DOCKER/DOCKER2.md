# crear un contenedor con mongodb 7 usando claude
### Crear la carpeta donde va estar el contenedor
```
mkdir -p ~/mongodb-docker && cd ~/mongodb-docker
```
### Crear un archivo de configuracion 
``` 
# vim ~/mongodb-docker/docker-compose.yml
services:
  mongodb:
    image: mongo:7.0
    container_name: mongodb
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: Admin1234!
      MONGO_INITDB_DATABASE: testdb
    ports:
      - "10.0.0.199:27017:27017"   # Expone solo en la IP de red Docker
    volumes:
      - mongo_data:/data/db
      - ./init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js:ro
    networks:
      - mongo_net

volumes:
  mongo_data:

networks:
  mongo_net:
    driver: bridge
```
### Crear un usuario y que se inicialice 
```
vim ~/mongodb-docker/init-mongo.js
db = db.getSiblingDB('testdb');
db.createUser({
  user: 'testuser',
  pwd: 'Test1234!',
  roles: [
    { role: 'readWrite', db: 'testdb' }
  ]
});
db.testcollection.insertOne({
  mensaje: 'Conexión exitosa a MongoDB',
  fecha: new Date(),
  servidor: '10.0.0.199'
});
```
 ### Levantar el contenedor
 ```
root@srv-test:~/mongodb-docker# docker compose up -d
[+] up 14/14
 ✔ Image mongo:7.0                  Pulled                                                                                                               22.2s
 ✔ Network mongodb-docker_mongo_net Created                                                                                                              0.0s
 ✔ Volume mongodb-docker_mongo_data Created                                                                                                              0.0s
 ✔ Container mongodb                Started       
docker ps
docker logs mongodb
```
### probar la conexion desde el contenedor
```
root@srv-test:~/mongodb-docker# docker exec -it mongodb mongosh --username admin --password Admin1234! --authenticationDatabase admin
Current Mongosh Log ID:	69b080333d3eaaf3ee8563b0
Connecting to:		mongodb://<credentials>@127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&authSource=admin&appName=mongosh+2.7.0
Using MongoDB:		7.0.30
Using Mongosh:		2.7.0

For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/


To help improve our products, anonymous usage data is collected and sent to MongoDB periodically (https://www.mongodb.com/legal/privacy-policy).
You can opt-out by running the disableTelemetry() command.

------
   The server generated these startup warnings when booting
   2026-03-10T20:32:18.219+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2026-03-10T20:32:19.127+00:00: Soft rlimits for open file descriptors too low
   2026-03-10T20:32:19.127+00:00: For customers running MongoDB 7.0, we suggest changing the contents of the following sysfsFile
------
```

### Verificar datos de prueba dentro de mongosh:
```
use testdb
db.testcollection.find().pretty()
test> show databases;
admin   100.00 KiB
config   12.00 KiB
local    72.00 KiB
testdb   40.00 KiB
test> use testdb
switched to db testdb
testdb> db.testcollection.find().pretty()
[
  {
    _id: ObjectId('69b07fd19cea56a6d58563b1'),
    mensaje: 'Conexión exitosa a MongoDB',
    fecha: ISODate('2026-03-10T20:32:17.112Z'),
    servidor: '10.0.0.199'
  }
]
testdb> exit
```

### extras que validar desde conexion de otro host pero con mongocompas se probo deesde debian 13 con la cadena de conexion que dio los datos
```
mongosh "mongodb://testuser:Test1234!@10.0.0.199:27017/testdb"
--> conexion con mongo compas --> mongodb://testuser:Test1234!@10.0.0.199:27017/testdb
```

### Crear una coleccion en mongodb tiene ese nombre no es una tabla con datos de pruebas
```
Conectarse mediante SH o mongo Compas
mongosh "mongodb://testuser:Test1234!@10.0.0.199:27017/testdb"

use testdb
db.empleados.insertMany([
  {
    _id: 1,
    nombre_completo: "Juan Pérez García",
    rol: "Desarrollador"
  },
  {
    _id: 2,
    nombre_completo: "María López Ruiz",
    rol: "Diseñadora"
  },
  {
    _id: 3,
    nombre_completo: "Carlos Mendoza Torres",
    rol: "Gerente"
  },
  {
    _id: 4,
    nombre_completo: "Ana Ramírez Vega",
    rol: "Analista"
  }
])
```

#### Crear un usuario de solo LECTURA y probar tambien la conexion en mongocompas
```
root@srv-test:/home/uti# docker exec -it mongodb mongosh --username admin --password Admin1234! --authenticationDatabase admin
Current Mongosh Log ID:	69b3068ecec3f7d8858563b0
Connecting to:		mongodb://<credentials>@127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&authSource=admin&appName=mongosh+2.7.0
Using MongoDB:		7.0.30
Using Mongosh:		2.7.0

For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

------
   The server generated these startup warnings when booting
   2026-03-11T20:29:00.160+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2026-03-11T20:29:01.230+00:00: Soft rlimits for open file descriptors too low
   2026-03-11T20:29:01.230+00:00: For customers running MongoDB 7.0, we suggest changing the contents of the following sysfsFile
------

test> use testdb
switched to db testdb
testdb> db.createUser({user:"visor_empleados",pwd:"Visor1234!",roles:[{role:"read",db:"testdb"}]})
{ ok: 1 }
testdb> db.empleados.find().pretty()
[
  { _id: 1, nombre_completo: 'Efrain Cuter', rol: 'IT' },
  { _id: 2, nombre_completo: 'Elvis Crespo', rol: 'Desarrollador' }
]
testdb> db.getUsers
db.getUsers

testdb> db.getUsers
[Function: getUsers] AsyncFunction {
  apiVersions: [ 0, 0 ],
  returnsPromise: true,
  serverVersions: [ '0.0.0', '999.999.999' ],
  topologies: [ 'ReplSet', 'Standalone', 'Sharded', 'LoadBalanced' ],
  returnType: { type: 'unknown', attributes: {} },
  deprecated: false,
  platforms: [ 'Compass', 'Browser', 'CLI' ],
  isDirectShellCommand: false,
  acceptsRawInput: false,
  shellCommandCompleter: undefined,
  newShellCommandCompleter: undefined,
  help: [Function (anonymous)] Help
}
testdb> db.getUsers()
{
  users: [
    {
      _id: 'testdb.testuser',
      userId: UUID('ce14161a-697b-4183-86db-e6d2cb4fd697'),
      user: 'testuser',
      db: 'testdb',
      roles: [ { role: 'readWrite', db: 'testdb' } ],
      mechanisms: [ 'SCRAM-SHA-1', 'SCRAM-SHA-256' ]
    },
    {
      _id: 'testdb.visor_empleados',
      userId: UUID('1fdc9df9-9d1c-4ab9-89a5-5c4a3dce8d71'),
      user: 'visor_empleados',
      db: 'testdb',
      roles: [ { role: 'read', db: 'testdb' } ],
      mechanisms: [ 'SCRAM-SHA-1', 'SCRAM-SHA-256' ]
    }
  ],
  ok: 1
}
testdb> 
```