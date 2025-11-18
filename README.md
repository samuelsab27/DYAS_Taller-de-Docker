# DYAS_Taller-de-Docker
# DYAS – Taller de Docker
**Por:** Julian Andres Vasquez Pedraza, Samuel Sabogal Espinel, Jorge steven doncel, Santiago Buendia

Este proyecto contiene el desarrollo completo del taller de Docker, donde se crean imágenes personalizadas, se construye una aplicación en Python con Flask + Redis y finalmente se despliega con Docker Compose utilizando Traefik como balanceador de carga.

El repositorio está dividido en dos ejercicios principales:

- **hello-world/** – Primer Dockerfile y creación de una imagen básica.
- **friendlyhello/** – Aplicación Flask con Redis, usando Docker Compose y balanceo de carga.

---

##  1. Ejercicio 1: Imagen personalizada “hello-world”

En esta carpeta se crea un build context minimalista con un archivo `hello` y un `Dockerfile` basado en la imagen `busybox`.

### Estructura
hello-world/
├─ Dockerfile
└─ hello

### Contenido del Dockerfile
```dockerfile
FROM busybox
COPY /hello /
RUN cat /hello
```
# 2. Ejercicio 2: Aplicación Flask con Redis (friendlyhello)

- La aplicación principal del taller. Consiste en un servicio web en Flask que muestra un saludo, el hostname del contenedor y un contador almacenado en Redis.

# Estructura
friendlyhello/
 ├─ app.py
 ├─ Dockerfile
 ├─ requirements.txt
 ├─ docker-compose.yaml
 └─ data/     # Volumen persistente de Redis

# Flujo de la aplicación

Flask inicia un servidor en el puerto 80.

Redis almacena el contador de visitas.

Traefik actúa como balanceador de carga, distribuyendo las peticiones entre múltiples instancias del servicio web.
# 2.1. Construcción de la imagen personalizada

La imagen fue creada con:

docker build -t <usuarioDockerHub>/friendlyhello .


Y subida al repositorio mediante:

docker push <usuarioDockerHub>/friendlyhello


En el docker-compose.yaml se usa esta imagen en lugar de un build: local (como lo pide el ejercicio).

# 2.2. Ejecución con Docker Compose
Levantar la aplicación con balanceo de carga
docker compose up -d --scale web=5


Este comando:

Crea y levanta Redis, Traefik y 5 contenedores web.

Permite que Traefik reparta tráfico entre las 5 instancias.

Acceder a la aplicación

Aplicación web: http://localhost:4000

Dashboard de Traefik: http://localhost:8080

Cada vez que recargas la página, el hostname cambia mostrando el contenedor que respondió.

# ¿Qué se hizo en este taller?

Creamos una imagen personalizada básica usando BusyBox.

Construimos una aplicación en Python con Flask, incluyendo dependencias externas.

Creamos un Dockerfile profesional para empaquetar la app dentro de un contenedor reproducible.

Configuramos Docker Compose para ejecutar múltiples servicios:

Aplicación web (Flask)

Base de datos Redis

Balanceador de carga Traefik

Publicamos una imagen en Docker Hub para usarla directamente en el compose.

Escalamos el servicio web ejecutando 5 contenedores simultáneamente.

Comprobamos el balanceo de carga, viendo cómo cambia el hostname del contenedor respondiendo cada petición.

# Conclusión

Este proyecto demuestra el ciclo completo de creación, empaquetado, despliegue y escalado de aplicaciones con Docker:

Desde una imagen mínima,

hasta una aplicación real distribuida y balanceada.

El taller integra de manera práctica Dockerfile, Docker Hub, Compose, Redis, Flask y Traefik.
