# LABORATORIO DE DOCKER
Laboratorio Docker del curso de DEVOPS
## Juan José Ruiz Rivero
## Mayo de 2026
<hr>

<hr>

### 1. Creando imágenes
### Paso 1
Abro Docker Desktop en mi equipo.

Desde la terminal ejecuto: `docker run -it ubuntu`

Cuando termina la ejecución me encuentro dentro del contenedor. Allí:<br><br/>
`apt-get update`<br><br/>
`apt-get install -y curl`<br><br/>
`curl --version`
<br><br/>
<img width="1209" height="322" alt="E1_C1" src="https://github.com/user-attachments/assets/e02103c0-fd06-4750-a45d-e971ecdf7859" />

#### Pregunta. ¿Con qué comando podrías guardar los cambios del contenedor como una nueva imagen?
`docker commit <ID_CONTENEDOR> <NOMBRE_IMAGEN>`

<br><br/>

### Paso 2 Dockerfile
Creo el siguiente Dockerfile:
<br><br/>
<img width="552" height="203" alt="E1_C2" src="https://github.com/user-attachments/assets/15259f05-6276-4f1e-85c4-eff200ceff45" />

Y ejecuto los comandos:

`docker build -t ubuntu_curl .`<br><br/>
`docker build -t ubuntu_curl -f Dockerfile_ubuntu_curl .`<br><br/>
`docker run ubuntu_curl curl --version`

<br><br/>
#### Pregunta. ¿Qué comando permite ver las capas de una imagen Docker?
`docker history <nombre_imagen>`

<hr>

### 3. Volúmenes persistentes
Creo primero el contenedor:<br><br/>
`docker run -d --name postgres-lab -e POSTGRES_PASSWORD=1234 -v mi-volumen-datos:/var/lib/postgresql/data postgres:17`

<br><br/>
Aunque el contenedor se detenga, al utilizar volumenes, los datos persisten.
<br><br/>
 
#### Comprobación
`docker logs postgres-lab`
<img width="1216" height="360" alt="E3_C1" src="https://github.com/user-attachments/assets/e0dab2cd-ffdd-4966-a521-5113b12a970b" />
<br><br/>
<br><br/>
<img width="1208" height="585" alt="E3_C2" src="https://github.com/user-attachments/assets/10911ee8-af9f-42a7-bb27-b977181c1b8e" />
<br><br/>
#### Para el contenedor
`docker stop postgres-lab`

#### Elimina el contenedor
`docker rm postgres-lab`

#### Crea un nuevo contenedor usando el mismo volumen
`docker run -d --name postgres-lab-2 -e POSTGRES_PASSWORD=1234 -v mi-volumen-datos:/var/lib/postgresql/data postgres:17`

`docker exec -it postgres-lab-2 bash`
`psql -U postgres`

#### Comprueba que los datos siguen existiendo.
`docker volume ls`

/var/lib/postgresql/data
 


CREATE TABLE items (
 id SERIAL PRIMARY KEY,
 name TEXT
);
 
Inserta un registro:

INSERT INTO items(name) VALUES ('item1');



<hr>

### 4. Bind mounts
Crea un archivo en tu máquina:

index.html
 
Ejemplo:

<h1>Hola Docker</h1>
 
Ejecuta un contenedor nginx:

mapea el puerto 80
monta el archivo en:
<!-- -->
 
/usr/share/nginx/html/index.html
 
Abre el navegador.

Pregunta:

¿Qué ocurre si modificas el archivo index.html en tu máquina?




<hr>

### 6. Creando redes privadas
Crea una red llamada:

my-net
 
Arranca dos contenedores ubuntu en esa red.

Instala ping si es necesario.

Desde un contenedor intenta hacer:

ping otro_contenedor
 
Pregunta

¿Los contenedores pueden comunicarse entre sí?


<hr>

### 9. Docker Compose --- Compartiendo volúmenes
Crea un fichero:

docker-compose.yml
 
Con dos servicios.

writer
Debe:

montar un volumen en /app/logs
escribir un timestamp cada 30 segundos
reader
Debe:

montar el volumen en modo solo lectura
mostrar el contenido en consola
