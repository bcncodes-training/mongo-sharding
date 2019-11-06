# mongo-sharding

# Creación de un cluster de desarrollo:
 __________
|          |
|  Router  |
| (Mongos) |
|__________|
     |	\   ____________
     |	 \ |            |
     |	  \|  Config    |
     |	  /|  Server    |
     |	 / |(ReplicaSet)|
     |  /  |____________|
 ____|_/______
|            |
|  Shards    |
|(Mongod o   |
|replica SET)|
|____________|



## Creación de los servidores de configuración:

1. Crear directorio de datos del servidor de configuración:
	>mkdir data/config/n1

2. Crear el servidor de configuración
 	>mongod --port 27023  --configsvr --replSet rs0 --dbpath /data/config/n1

3. Conectar un cliente mongo. 
	>mongo --host localhost –-port 27023

4. Iniciar el replica Set:
	>rs.initiate()

## Creación de routers:
	 >mongos --configdb rs0/localhost:27023 --port 27000

## Creación de las réplicas de sharding:
#PRIMER SHARD
1. Crear directorio de datos del servidor de configuración:
	>mkdir data/shard/s1

2. Crear el servidor de configuración
	>mongod --port 27030 --shardsvr --replSet rs1  --dbpath /data/shard/s1

3. Conectar un cliente mongo. 
	>mongo --host localhost –-port 27030

4. Iniciar el replica Set:
	>rs.initiate()
#SEGUNDO SHARD
1. Crear directorio de datos del servidor de configuración:
	>mkdir data/shard/s2

2. Crear el servidor de configuración
	>mongod --port 27031 --shardsvr --replSet rs2  --dbpath /data/shard/s2

3. Conectar un cliente mongo. 
	>mongo --host localhost –-port 27031

4. Iniciar el replica Set:
	>rs.initiate()

## Añadir los shards al cluster

1. Conectar un cliente mongo a mongos. 
	>mongo --host localhost –-port 27000
2. Añadir los shards.
	>sh.addShard( "rs1/localhost:27030")
	>sh.addShard( "rs2/localhost:27031")
3. Comprobar el cluster
	>sh.status()
3. Activar los shards para la bd.
	>sh.enable("<database>")
4. Activar la distribución de una colección
	>sh.shardCollection("people.people",{_id:"hashed"})
## Restaurar backup bd people:
	mongorestore --gzip --host localhost --port 27000 --db people

## Comprobar estado de los shardings:
	>mongo --host localhost –-port 27000
	>sh.status()	
