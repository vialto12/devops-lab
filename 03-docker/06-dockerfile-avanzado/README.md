# Dockerfile avanzado

## Objetivo
Crear una imagen Docker personalizada a partir de una imagen base
## 1. WORKDIR + COPY + CMD
Dockerfile
FROM alpine:latest

WORKDIR /app

COPY mensaje.txt .

CMD ["cat", "mensaje.txt"]

Como WORKDIR establece /app, el . de COPY significa:
/app
Por tanto:
menaje.txt --> /app/mensaje.txt
## 2. RUN vs CMD
RUN → durante el BUILD de la imagen
CMD → cuando se ARRANCA el contenedor

Ejemplo:
RUN echo "Este mensaje aparece durante el build"
- Se ve cuando haces docker build...
Mientras que:
CMD ["echo", "Este mensaje aparece cuando ejecuto el contenedor"]
- Se ejecuta cuando hacemos docker run ...
## 4. EXPOSE
EXPOSE 80
Esta aplicación dentro del contenedor utiliza el puerto 80
HOST        CONTENEDOR
8080   →    80
## 5. Construcción de imágenes
Construir una imagen Docker a partir del Dockerfile utilizando docker build
docker build -t devops-nginx .
## 6. Ejecución de contenedores
docker run -d --name devops-nginx -p 8080:80 devops-nginx
docker run       → crear y arrancar
-d               → segundo plano
--name           → nombre del contenedor
-p 8080:80       → publicar puerto
devops-nginx     → imagen utilizada
## 7. Comprobación
docker ps
docker logs devops-nginx --> consultar logs
http://localhost:8080 --> aplicación
## 8. Lo aprendido
- Crear imágenes mediante Dockerfile.
- Utilizar una imagen base.
- Copiar archivos dentro de una imagen.
- Establecer un directorio de trabajo.
- Diferenciar RUN y CMD.
- Declarar puertos con EXPOSE.
- Publicar puertos con `-p`.
- Crear y ejecutar contenedores.
- Consultar los logs de un contenedor.
## 9. Comandos utilizados
docker build
docker run
docker ps
docker logs
docker images
//
FROM   → de dónde parto
COPY   → qué meto dentro
RUN    → qué ejecuto al construir
CMD    → qué ejecuto al arrancar
EXPOSE → qué puerto utiliza la aplicación
