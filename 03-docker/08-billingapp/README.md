# BillingApp - Docker Compose

## Objetivo

Ejecutar una aplicación Spring Boot dentro de un contenedor Docker utilizando Docker Compose.

La aplicación se llama `devopslab` y tiene un endpoint que devuelve:

`Hola desde DevOps Lab`

---

## 1. Aplicación Spring Boot

La aplicación se encuentra en:

app/devopslab/

Dentro tenemos el código de la aplicación, el `pom.xml` y el `Dockerfile`.

El endpoint creado es:

GET /api/message

Y devuelve:

Hola desde DevOps Lab

---

## 2. Crear el JAR

Primero se compila la aplicación con Maven para generar el archivo `.jar`.

El archivo generado se encuentra en:

target/devopslab-0.0.1-SNAPSHOT.jar

Este archivo no se sube a Git porque `target/` está incluido en `.gitignore`.

---

## 3. Dockerfile

El Dockerfile utilizado es:

FROM eclipse-temurin:21-jre

WORKDIR /app

COPY target/devopslab-0.0.1-SNAPSHOT.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]

### Qué hace cada instrucción

**FROM**

Partimos de una imagen que contiene Java 21:

eclipse-temurin:21-jre

**WORKDIR**

Establece `/app` como directorio de trabajo dentro del contenedor.

**COPY**

Copia el `.jar` generado por Maven dentro del contenedor y lo guarda como:

/app/app.jar

**EXPOSE**

Declara que la aplicación utiliza el puerto `8080` dentro del contenedor.

**ENTRYPOINT**

Indica que al iniciar el contenedor se ejecutará:

java -jar app.jar

---

## 4. Crear la imagen Docker

Desde:

app/devopslab/

ejecutamos:

docker build -t devopslab:1.0 .

Esto crea la imagen:

devopslab:1.0

La imagen contiene nuestra aplicación Spring Boot preparada para ejecutarse.

---

## 5. Ejecutar la aplicación con Docker

Antes de utilizar Docker Compose, probamos la imagen directamente con:

docker run -d --name devopslab -p 8080:8080 devopslab:1.0

Esto conecta:

HOST 8080 → CONTENEDOR 8080

Y permite acceder a la aplicación desde:

http://localhost:8080

El endpoint de la aplicación es:

http://localhost:8080/api/message

Comprobación:

curl http://localhost:8080/api/message

Resultado:

Hola desde DevOps Lab

---

## 6. Docker Compose

Creamos un archivo:

compose.yaml

con el siguiente contenido:

services:
  devopslab:
    image: devopslab:1.0
    ports:
      - "8080:8080"

Aquí no estamos creando una nueva imagen.

Estamos indicando a Docker Compose que utilice la imagen que ya hemos creado:

devopslab:1.0

---

## 7. Validar Docker Compose

Antes de levantar el servicio podemos comprobar que la configuración es correcta:

docker compose config

Docker Compose interpreta:

image: devopslab:1.0

y:

8080:8080

También crea automáticamente una red para el proyecto:

devopslab_default

---

## 8. Levantar la aplicación con Docker Compose

Para levantar el servicio utilizamos:

docker compose up -d

Compose se encarga automáticamente de:

- Crear la red.
- Crear el contenedor.
- Utilizar la imagen `devopslab:1.0`.
- Configurar el puerto.
- Arrancar la aplicación.

Podemos comprobar el estado con:

docker compose ps

El contenedor creado tiene un nombre generado automáticamente por Compose:

devopslab-devopslab-1

El nombre sigue aproximadamente esta estructura:

PROYECTO-SERVICIO-NÚMERO

En nuestro caso:

devopslab-devopslab-1
     │        │       │
     │        │       └── número de instancia
     │        └────────── servicio
     └─────────────────── proyecto

---

## 9. Comprobación

Comprobamos que la aplicación funciona:

curl http://localhost:8080/api/message

Resultado:

Hola desde DevOps Lab

También podemos comprobar los logs:

docker compose logs

Los logs muestran que Spring Boot inicia Tomcat en el puerto `8080`.

---

## 10. Parar la aplicación

Para detener y eliminar los recursos creados por Docker Compose:

docker compose down

Para volver a levantar la aplicación:

docker compose up -d

---

## 11. Dockerfile vs Docker Compose

Es importante diferenciar ambos archivos.

### Dockerfile

Define cómo construir la imagen:

Dockerfile
    ↓
docker build
    ↓
devopslab:1.0

### compose.yaml

Define cómo ejecutar y configurar el servicio:

compose.yaml
    ↓
docker compose up -d
    ↓
contenedor

Por tanto:

Dockerfile → construye la imagen

Docker Compose → ejecuta y configura los contenedores

---

## 12. Lo aprendido

- Una aplicación Spring Boot puede ejecutarse dentro de Docker.
- Maven genera el archivo `.jar`.
- El Dockerfile utiliza ese `.jar` para crear una imagen.
- La imagen creada se llama `devopslab:1.0`.
- Docker Compose puede utilizar una imagen que ya existe.
- No es necesario escribir todos los parámetros de `docker run` cada vez.
- `compose.yaml` guarda la configuración del servicio.
- Docker Compose crea automáticamente una red para los servicios.
- Docker Compose genera automáticamente el nombre del contenedor.
- `docker compose up -d` crea y arranca los servicios.
- `docker compose down` detiene y elimina los recursos creados por Compose.
- El puerto `8080` del host se conecta con el puerto `8080` del contenedor.
- La aplicación se puede comprobar mediante el endpoint `/api/message`.

---

## 13. Comandos utilizados

# Comprobar la estructura del proyecto
tree app/devopslab -L 3

# Crear la imagen
docker build -t devopslab:1.0 .

# Ver las imágenes
docker images

# Ejecutar manualmente el contenedor
docker run -d --name devopslab -p 8080:8080 devopslab:1.0

# Comprobar contenedores
docker ps

# Probar la aplicación
curl http://localhost:8080/api/message

# Validar Docker Compose
docker compose config

# Levantar el servicio
docker compose up -d

# Comprobar el servicio
docker compose ps

# Ver los logs
docker compose logs

# Detener y eliminar los recursos de Compose
docker compose down

---

## 14. Resumen

El flujo completo realizado ha sido:

Código Spring Boot
        ↓
      Maven
        ↓
      .jar
        ↓
   Dockerfile
        ↓
   docker build
        ↓
   devopslab:1.0
        ↓
   compose.yaml
        ↓
docker compose up -d
        ↓
    Contenedor
        ↓
 localhost:8080
        ↓
 /api/message
        ↓
Hola desde DevOps Lab
