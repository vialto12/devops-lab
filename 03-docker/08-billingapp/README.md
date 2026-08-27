# BillingApp - Spring Boot + Docker

## Objetivo

Dockerizar una aplicación Spring Boot y ejecutarla dentro de un contenedor Docker.

## 1. Aplicación Spring Boot

La aplicación está creada con Spring Boot y utiliza Java 21.

La aplicación tiene un endpoint:

    GET /api/message

Que devuelve:

    Hola desde DevOps Lab

Para probar la aplicación directamente con Spring Boot:

    curl http://localhost:8080/api/message

## 2. Generar el JAR

Antes de crear la imagen Docker necesitamos tener generado el archivo `.jar` de la aplicación.

El JAR generado es:

    target/devopslab-0.0.1-SNAPSHOT.jar

## 3. Dockerfile

El Dockerfile utilizado es:

    FROM eclipse-temurin:21-jre

    WORKDIR /app

    COPY target/devopslab-0.0.1-SNAPSHOT.jar app.jar

    EXPOSE 8080

    ENTRYPOINT ["java", "-jar", "app.jar"]

### Explicación

**FROM** → utilizamos una imagen con Java 21 para ejecutar la aplicación.

**WORKDIR** → establece `/app` como directorio de trabajo dentro del contenedor.

**COPY** → copia el JAR generado dentro del contenedor y lo guarda como `app.jar`.

**EXPOSE** → indica que la aplicación utiliza el puerto 8080 dentro del contenedor.

**ENTRYPOINT** → ejecuta la aplicación Java cuando se inicia el contenedor.

## 4. Construcción de la imagen

Desde la carpeta de la aplicación:

    cd ~/proyectos/devops-lab/app/devopslab

Creamos la imagen:

    docker build -t devopslab:1.0 .

El comando `docker build` crea una imagen a partir del Dockerfile.

La imagen creada se llama:

    devopslab:1.0

## 5. Ejecución del contenedor

Ejecutamos la imagen:

    docker run -d --name devopslab -p 8080:8080 devopslab:1.0

### Explicación

**docker run** → crea y ejecuta un contenedor.

**-d** → ejecuta el contenedor en segundo plano.

**--name devopslab** → asigna el nombre `devopslab` al contenedor.

**-p 8080:8080** → conecta el puerto 8080 del ordenador con el puerto 8080 del contenedor.

**devopslab:1.0** → indica qué imagen utilizamos.

## 6. Problema encontrado

Al intentar crear el contenedor apareció el siguiente error:

    Conflict. The container name "/devopslab" is already in use

Esto ocurría porque ya existía un contenedor antiguo llamado `devopslab`.

Comprobamos los contenedores:

    docker ps -a --filter "name=devopslab"

El contenedor antiguo estaba detenido y utilizaba una imagen antigua.

## 7. Solución

Eliminamos el contenedor antiguo:

    docker rm devopslab

Después creamos el nuevo contenedor utilizando la imagen actual:

    docker run -d --name devopslab -p 8080:8080 devopslab:1.0

## 8. Comprobación

Comprobamos que el contenedor está funcionando:

    docker ps

El resultado mostró:

    devopslab:1.0
    0.0.0.0:8080->8080/tcp

Después comprobamos la API:

    curl http://localhost:8080/api/message

Resultado:

    Hola desde DevOps Lab

Esto confirma que la aplicación Spring Boot está funcionando correctamente dentro de Docker.

## 9. Logs

Para consultar los logs del contenedor:

    docker logs devopslab

Los logs mostraron que Spring Boot se inició correctamente y que Tomcat estaba escuchando en el puerto 8080.

También podemos seguir los logs en tiempo real:

    docker logs -f devopslab

Para salir de los logs:

    Ctrl + C

## 10. Lo aprendido

- Una aplicación Spring Boot puede ejecutarse dentro de un contenedor Docker.
- `docker build` crea una imagen.
- `docker run` crea y ejecuta un contenedor a partir de una imagen.
- `EXPOSE` indica el puerto utilizado dentro del contenedor.
- `-p` conecta el puerto del ordenador con el puerto del contenedor.
- Los contenedores tienen un nombre único.
- Un contenedor detenido sigue existiendo y puede impedir reutilizar su nombre.
- `docker logs` permite consultar lo que ocurre dentro del contenedor.
- La aplicación puede comprobarse mediante una petición HTTP.

## 11. Comandos utilizados

    docker build -t devopslab:1.0 .

    docker images

    docker run -d --name devopslab -p 8080:8080 devopslab:1.0

    docker ps

    docker ps -a --filter "name=devopslab"

    docker rm devopslab

    docker logs devopslab

    docker logs -f devopslab

    curl http://localhost:8080/api/message
