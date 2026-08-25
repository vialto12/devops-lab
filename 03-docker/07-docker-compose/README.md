# Docker Compose

## Objetivo

Definir y gestionar uno o varios servicios mediante un archivo YAML y poder levantarlos con un solo comando.

---

## 1. Crear `compose.yaml`

Creamos un archivo `compose.yaml` donde definimos el servicio de Nginx:

*yaml*
services:
  nginx:
    image: nginx:alpine
    ports:
      - "8080:80"

services → define los servicios que vamos a gestionar.
nginx → nombre que damos al servicio.
image → imagen que utilizará el contenedor.
ports → establece el mapeo entre el puerto del host y el puerto del contenedor.

## 2. Validar la configuración

#docker compose config
Este comando no levanta ningún contenedor. Simplemente valida y muestra la configuración que Docker Compose ha interpretado.

## 3. Levantar el servicio

#docker compose up -d
La opción -d significa que el servicio se ejecuta en segundo plano.
Una de las ventajas de Docker Compose es que no tenemos que escribir todos los parámetros de docker run cada vez.

## 4. Problema encontrado

Al intentar levantar Docker Compose encontramos el siguiente error:
#Bind for 0.0.0.0:8080 failed: port is already allocated

El problema era que ya teníamos otro contenedor llamado "devops-nginx" utilizando el puerto 8080 del host.
Por eso Docker Compose no podía publicar el puerto 8080

## 5. Solución

Primero identificamos el contenedor que estaba utilizando el puerto:
#docker ps

Después paramos el contenedor:
#docker stop devops-nginx

Como el contenedor de Docker Compose se había creado durante el intento anterior, lo recreamos para aplicar correctamente toda la configuración:
#docker compose down

Y posteriormente:
#docker compose up -d

De esta forma Docker Compose creó de nuevo el contenedor aplicando el puerto correctamente
## 6. Comprobación

Comprobamos que el servicio está funcionando:
#docker compose ps

Para comprobar los logs del servicio:
#docker compose logs nginx

Finalmente comprobamos que Nginx responde correctamente:
#curl http://localhost:8080
## 7. Comandos utilizados

Validar configuración
#docker compose config

Levantar servicios
#docker compose up -d

Ver servicios de Compose
#docker compose ps

Ver logs
#docker compose logs nginx

Parar un contenedor
#docker stop devops-nginx

Detener y eliminar los recursos del proyecto Compose
#docker compose down

Comprobar los puertos de un contenedor
#docker port 07-docker-compose-nginx-1

Inspeccionar un contenedor
#docker inspect 07-docker-compose-nginx-1

Comprobar que el servicio responde
#curl http://localhost:8080

## 8. Lo aprendido
Docker Compose permite definir servicios mediante un archivo YAML.
Podemos gestionar los servicios utilizando comandos de Docker Compose.
docker compose config permite validar la configuración del archivo YAML.
docker compose up -d crea y levanta los servicios en segundo plano.
docker compose ps permite comprobar el estado de los servicios del proyecto.
docker compose down detiene y elimina los contenedores y redes creados por Compose.
Los puertos del host no pueden ser utilizados simultáneamente por varios contenedores.
Si modificamos una configuración y necesitamos que un contenedor existente la aplique correctamente, puede ser necesario recrearlo.
Docker Compose facilita la gestión de varios servicios y evita tener que escribir manualmente comandos docker run complejos.
Compose también crea automáticamente una red para los servicios definidos en el proyecto.
