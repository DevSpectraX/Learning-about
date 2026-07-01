## 🐳 Guía Completa de Comandos de Docker

---

## 📦 1. Gestión de Imágenes

Las imágenes son las "plantillas" desde las que se crean los contenedores.

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `docker pull` | Descarga una imagen del registro (Docker Hub) | `docker pull postgres:15` |
| `docker images` | Lista todas las imágenes locales | `docker images` |
| `docker build` | Construye una imagen desde un Dockerfile | `docker build -t mi-app:1.0 .` |
| `docker rmi` | Elimina una imagen | `docker rmi postgres:15` |
| `docker tag` | Etiqueta una imagen con otro nombre | `docker tag mi-app:1.0 usuario/mi-app:latest` |
| `docker push` | Sube una imagen a Docker Hub | `docker push usuario/mi-app:latest` |
| `docker image prune` | Elimina imágenes no utilizadas | `docker image prune -a` |

### 💡 Ejemplo práctico:
```bash
# Descargar PostgreSQL
docker pull postgres:15

# Ver todas las imágenes
docker images

# Construir tu propia imagen (desde un Dockerfile)
docker build -t juego-combate:1.0 .
```

---

## 🐳 2. Gestión de Contenedores

Los contenedores son las instancias en ejecución de las imágenes.

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `docker run` | Crea y arranca un contenedor | `docker run -d --name mi-db postgres:15` |
| `docker ps` | Lista contenedores en ejecución | `docker ps` |
| `docker ps -a` | Lista TODOS los contenedores (incluso parados) | `docker ps -a` |
| `docker start` | Arranca un contenedor parado | `docker start mi-db` |
| `docker stop` | Para un contenedor | `docker stop mi-db` |
| `docker restart` | Reinicia un contenedor | `docker restart mi-db` |
| `docker rm` | Elimina un contenedor parado | `docker rm mi-db` |
| `docker rm -f` | Fuerza la eliminación (aunque esté corriendo) | `docker rm -f mi-db` |
| `docker exec` | Ejecuta un comando dentro de un contenedor | `docker exec -it mi-db bash` |
| `docker attach` | Se conecta al contenedor en ejecución | `docker attach mi-db` |
| `docker logs` | Muestra los logs del contenedor | `docker logs mi-db` |
| `docker logs -f` | Muestra logs en tiempo real (follow) | `docker logs -f mi-db` |

### 💡 Ejemplo práctico:
```bash
# Ejecutar PostgreSQL con variables de entorno
docker run -d \
  --name mi-postgres \
  -e POSTGRES_PASSWORD=miPassword123 \
  -e POSTGRES_USER=admin \
  -e POSTGRES_DB=miBaseDeDatos \
  -p 5432:5432 \
  postgres:15

# Ver contenedores corriendo
docker ps

# Ver logs en tiempo real
docker logs -f mi-postgres

# Entrar al contenedor
docker exec -it mi-postgres bash

# Parar y eliminar
docker stop mi-postgres
docker rm mi-postgres
```

### 🔑 Opciones importantes de `docker run`:

| Opción | Descripción | Ejemplo |
|--------|-------------|---------|
| `-d` | Modo "detached" (en segundo plano) | `-d` |
| `--name` | Nombre personalizado del contenedor | `--name mi-db` |
| `-p` | Mapeo de puertos (host:contenedor) | `-p 5432:5432` |
| `-e` | Variable de entorno | `-e POSTGRES_PASSWORD=123` |
| `-v` | Volumen (persistencia de datos) | `-v datos:/var/lib/postgresql/data` |
| `--rm` | Elimina el contenedor al pararlo | `--rm` |
| `-it` | Modo interactivo con terminal | `-it` |

---

## 💾 3. Gestión de Volúmenes

Los volúmenes permiten **persistir datos** aunque el contenedor se elimine.

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `docker volume create` | Crea un volumen | `docker volume create datos-db` |
| `docker volume ls` | Lista todos los volúmenes | `docker volume ls` |
| `docker volume inspect` | Muestra detalles de un volumen | `docker volume inspect datos-db` |
| `docker volume rm` | Elimina un volumen | `docker volume rm datos-db` |
| `docker volume prune` | Elimina volúmenes no utilizados | `docker volume prune` |

### 💡 Ejemplo práctico con PostgreSQL:
```bash
# Crear volumen para la base de datos
docker volume create postgres-data

# Usar el volumen en un contenedor
docker run -d \
  --name mi-postgres \
  -e POSTGRES_PASSWORD=123 \
  -v postgres-data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15

# Aunque elimines el contenedor, los datos persisten en el volumen
docker rm -f mi-postgres
docker run -d --name mi-postgres-nuevo \
  -e POSTGRES_PASSWORD=123 \
  -v postgres-data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15
# ¡Los datos siguen ahí!
```

---

## 🌐 4. Gestión de Redes

Las redes permiten la comunicación entre contenedores.

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `docker network create` | Crea una red | `docker network create mi-red` |
| `docker network ls` | Lista todas las redes | `docker network ls` |
| `docker network inspect` | Muestra detalles de una red | `docker network inspect mi-red` |
| `docker network connect` | Conecta un contenedor a una red | `docker network connect mi-red mi-db` |
| `docker network disconnect` | Desconecta un contenedor | `docker network disconnect mi-red mi-db` |
| `docker network rm` | Elimina una red | `docker network rm mi-red` |
| `docker network prune` | Elimina redes no utilizadas | `docker network prune` |

### 💡 Ejemplo práctico:
```bash
# Crear una red para tu aplicación
docker network create red-juego

# Ejecutar PostgreSQL en esa red
docker run -d \
  --name postgres \
  --network red-juego \
  -e POSTGRES_PASSWORD=123 \
  postgres:15

# Ejecutar tu app Java conectada a la misma red
docker run -d \
  --name mi-app \
  --network red-juego \
  -e DB_HOST=postgres \
  mi-app:1.0
# Desde mi-app puedes conectar a PostgreSQL usando "postgres" como hostname
```

---

## 📋 5. Docker Compose

Permite definir y ejecutar aplicaciones multi-contenedor con un archivo YAML.

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `docker compose up` | Crea y arranca los servicios | `docker compose up` |
| `docker compose up -d` | Arranca en segundo plano (detached) | `docker compose up -d` |
| `docker compose down` | Para y elimina contenedores, redes | `docker compose down` |
| `docker compose down -v` | También elimina los volúmenes | `docker compose down -v` |
| `docker compose ps` | Lista los servicios | `docker compose ps` |
| `docker compose logs` | Muestra logs de todos los servicios | `docker compose logs` |
| `docker compose logs -f` | Logs en tiempo real | `docker compose logs -f` |
| `docker compose build` | Reconstruye las imágenes | `docker compose build` |
| `docker compose restart` | Reinicia los servicios | `docker compose restart` |
| `docker compose stop` | Para los servicios sin eliminarlos | `docker compose stop` |
| `docker compose start` | Arranca servicios parados | `docker compose start` |
| `docker compose exec` | Ejecuta comando en un servicio | `docker compose exec db bash` |

### 💡 Ejemplo de `docker-compose.yml`:

```yaml
version: '3.8'

services:
  # Servicio de base de datos
  db:
    image: postgres:15
    container_name: mi-postgres
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: miPassword123
      POSTGRES_DB: juego_db
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - red-juego

  # Servicio de tu aplicación Java
  app:
    build: .
    container_name: mi-app-java
    ports:
      - "8080:8080"
    environment:
      - DB_HOST=db
      - DB_PORT=5432
      - DB_NAME=juego_db
    depends_on:
      - db
    networks:
      - red-juego

volumes:
  postgres-data:

networks:
  red-juego:
```

### 💻 Uso:
```bash
# Levantar todo
docker compose up -d

# Ver logs
docker compose logs -f

# Parar todo
docker compose down
```

---

## 🔍 6. Comandos de Depuración

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `docker logs` | Ver logs del contenedor | `docker logs mi-db` |
| `docker logs -f` | Logs en tiempo real | `docker logs -f mi-db` |
| `docker logs --tail 50` | Últimas 50 líneas | `docker logs --tail 50 mi-db` |
| `docker inspect` | Información detallada (JSON) | `docker inspect mi-db` |
| `docker top` | Procesos en ejecución | `docker top mi-db` |
| `docker stats` | Uso de recursos (CPU, RAM) | `docker stats` |
| `docker exec -it` | Entrar al contenedor | `docker exec -it mi-db bash` |
| `docker cp` | Copiar archivos desde/hacia contenedor | `docker cp mi-db:/data/file.txt ./` |

### 💡 Ejemplo práctico:
```bash
# Ver qué está consumiendo recursos
docker stats

# Entrar a PostgreSQL para ejecutar consultas
docker exec -it mi-postgres psql -U admin -d miBaseDeDatos

# Copiar un archivo desde el contenedor a tu PC
docker cp mi-postgres:/var/lib/postgresql/data/backup.sql ./backup.sql

# Ver información detallada (IP, puertos, volúmenes)
docker inspect mi-postgres
```

---

## 🧹 7. Limpieza del Sistema

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `docker system prune` | Elimina datos no utilizados | `docker system prune` |
| `docker system prune -a` | Más agresivo (incluye imágenes) | `docker system prune -a` |
| `docker system df` | Muestra uso de disco | `docker system df` |
| `docker container prune` | Elimina contenedores parados | `docker container prune` |
| `docker image prune` | Elimina imágenes "colgadas" | `docker image prune` |
| `docker volume prune` | Elimina volúmenes no usados | `docker volume prune` |
| `docker network prune` | Elimina redes no usadas | `docker network prune` |

### 💡 Ejemplo práctico:
```bash
# Ver cuánto espacio usa Docker
docker system df

# Limpieza completa (¡CUIDADO!)
docker system prune -a --volumes
```

---

## 🎯 8. Comandos Esenciales (Los más usados)

Si solo puedes memorizar unos pocos, que sean estos:

```bash
# 📥 Descargar imagen
docker pull postgres:15

# 🚀 Ejecutar contenedor
docker run -d --name mi-db -p 5432:5432 -e POSTGRES_PASSWORD=123 postgres:15

# 👀 Ver contenedores corriendo
docker ps

# 🛑 Parar contenedor
docker stop mi-db

# 🗑️ Eliminar contenedor
docker rm mi-db

# 📜 Ver logs
docker logs -f mi-db

# 🚪 Entrar al contenedor
docker exec -it mi-db bash

# 🧹 Limpieza
docker system prune
```

---

## 📊 Resumen Visual

```
┌─────────────────────────────────────────────────────────┐
│                    CICLO DE VIDA                         │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  docker pull ──→ docker run ──→ docker ps              │
│       │              │             │                    │
│       │              ▼             ▼                    │
│       │         [contenedor]   docker logs              │
│       │              │             │                    │
│       │              ▼             ▼                    │
│       │         docker stop   docker exec               │
│       │              │                                  │
│       │              ▼                                  │
│       │         docker rm                               │
│       │                                                 │
│       └──── docker images ──→ docker rmi               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 💡 Consejos Pro

1. **Usa alias** para comandos frecuentes en tu `.bashrc`:
   ```bash
   alias d='docker'
   alias dps='docker ps'
   alias dimg='docker images'
   alias dc='docker compose'
   ```

2. **Autocompletado**: Activa el autocompletado de Docker en Bash:
   ```bash
   sudo apt install docker-compose-plugin
   ```

3. **Documentación oficial**: [docs.docker.com](https://docs.docker.com/)

4. **Ayuda en cualquier comando**:
   ```bash
   docker run --help
   docker compose --help
   ```