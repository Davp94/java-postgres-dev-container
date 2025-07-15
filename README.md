# Comandos Docker y Docker Compose

## **Docker**

### **Comandos Básicos**
- **Ver información de Docker**:
  ```
  docker info
  ```
- **Listar imágenes disponibles**:
  ```
  docker images
  ```
- **Listar contenedores (en ejecución)**:
  ```
  docker ps
  ```
- **Listar todos los contenedores (incluidos detenidos)**:
  ```
  docker ps -a
  ```

### **Gestión de Imágenes**
- **Descargar una imagen desde Docker Hub**:
  ```
  docker pull nombre_imagen:etiqueta
  ```
  Ejemplo: `docker pull nginx:latest`
- **Construir una imagen desde un Dockerfile**:
  ```
  docker build -t nombre_imagen:etiqueta .
  ```
  Ejemplo: `docker build -t mi_app:1.0 .`
- **Eliminar una imagen**:
  ```
  docker rmi nombre_imagen:etiqueta
  ```
  Ejemplo: `docker rmi nginx:latest`

### **Gestión de Contenedores**
- **Ejecutar un contenedor**:
  ```
  docker run [opciones] nombre_imagen
  ```
  Ejemplo: `docker run -d -p 8080:80 nginx`
  - `-d`: Ejecutar en modo detached (en segundo plano).
  - `-p`: Mapear puertos (host:contenedor).
  - `--name`: Asignar un nombre al contenedor.
- **Iniciar un contenedor detenido**:
  ```
  docker start nombre_contenedor
  ```
- **Detener un contenedor**:
  ```
  docker stop nombre_contenedor
  ```
- **Eliminar un contenedor**:
  ```
  docker rm nombre_contenedor
  ```
- **Ver logs de un contenedor**:
  ```
  docker logs nombre_contenedor
  ```
- **Acceder a un contenedor en ejecución**:
  ```
  docker exec -it nombre_contenedor bash
  ```

### **Gestión de Volúmenes**
- **Listar volúmenes**:
  ```
  docker volume ls
  ```
- **Crear un volumen**:
  ```
  docker volume create nombre_volumen
  ```
- **Eliminar un volumen**:
  ```
  docker volume rm nombre_volumen
  ```
- **Montar un volumen en un contenedor**:
  ```
  docker run -v nombre_volumen:/ruta/en/contenedor nombre_imagen
  ```

### **Gestión de Redes**
- **Listar redes**:
  ```
  docker network ls
  ```
- **Crear una red**:
  ```
  docker network create nombre_red
  ```
- **Conectar un contenedor a una red**:
  ```
  docker network connect nombre_red nombre_contenedor
  ```

### **Limpieza**
- **Eliminar contenedores detenidos**:
  ```
  docker container prune
  ```
- **Eliminar imágenes no utilizadas**:
  ```
  docker image prune
  ```
- **Eliminar volúmenes no utilizados**:
  ```
  docker volume prune
  ```
- **Eliminar todo lo no utilizado**:
  ```
  docker system prune -a
  ```

## **Docker Compose**

### **Comandos Básicos**
- **Iniciar servicios definidos en docker-compose.yml**:
  ```
  docker compose up
  ```
  - `-d`: Ejecutar en modo detached.
- **Detener y eliminar servicios**:
  ```
  docker compose down
  ```
- **Listar servicios en ejecución**:
  ```
  docker compose ps
  ```
- **Ver logs de los servicios**:
  ```
  docker compose logs
  ```
- **Construir o reconstruir servicios**:
  ```
  docker compose build
  ```

### **Estructura de docker-compose.yml**
Un archivo `docker-compose.yml` típico puede verse así:

```yaml
version: "3.8"
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - mi_red
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: ejemplo
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - mi_red
volumes:
  db_data:
networks:
  mi_red:
    driver: bridge
```

### **Explicación de Campos Comunes**
- **`version`**: Versión del formato de Docker Compose.
- **`services`**: Define los contenedores (servicios) que se ejecutarán.
  - `image`: Imagen a usar (puede ser de Docker Hub o local).
  - `build`: Ruta al Dockerfile para construir la imagen.
  - `ports`: Mapeo de puertos (host:contenedor).
  - `volumes`: Montaje de volúmenes para persistencia.
  - `environment`: Variables de entorno.
  - `networks`: Redes a las que se conecta el servicio.
- **`volumes`**: Define volúmenes nombrados.
- **`networks`**: Define redes personalizadas.

### **Comandos Útiles de Docker Compose**
- **Ejecutar un comando en un servicio**:
  ```
  docker compose exec nombre_servicio comando
  ```
  Ejemplo: `docker-compose exec web bash`
- **Reconstruir un servicio específico**:
  ```
  docker compose up --build nombre_servicio
  ```
- **Escalar un servicio**:
  ```
  docker compose up --scale nombre_servicio=2
  ```
- **Pausar servicios**:
  ```
  docker compose pause
  ```
- **Reanudar servicios pausados**:
  ```
  docker compose unpause
  ```
