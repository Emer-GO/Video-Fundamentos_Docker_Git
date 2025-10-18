# Video-Fundamentos_Docker_Git

# Día 1 — Fundamentos + Entorno Docker / Git

## 🎯 Objetivo General
Comprender los fundamentos de la arquitectura de microservicios y preparar el entorno base de trabajo usando Docker Compose y Git, de modo que cada servicio pueda levantarse y gestionarse de forma independiente.

---

## 🧠 Conceptos Clave

- Diferencia entre arquitectura monolítica y microservicios.
- Principios: autonomía, responsabilidad única, acoplamiento flexible, escalabilidad y observabilidad.
- Estructura de proyecto multi-servicio.
- Uso de Docker + Docker Compose para levantar contenedores.
- Control de versiones con Git (ramas main y staging).

---

## 📁 Estructura del Proyecto

microservices-lab/
├── auth-service/ # Servicio de autenticación y tokens JWT
├── blog-service/ # Gestión de publicaciones, autores y categorías
├── email-service/ # Envío de correos y notificaciones
├── frontend/ # Interfaz web con React
├── reverse-proxy/ # Proxy inverso / gateway local
├── docker-compose.yml # Configuración de contenedores base
├── .env.example # Variables de entorno de ejemplo
└── README.md # Documentación general del proyecto

yaml
Copiar código

---

## ⚙️ Servicios Base

| Servicio     | Imagen Docker   | Puerto | Descripción                        |
|--------------|-----------------|--------|------------------------------------|
| PostgreSQL   | postgres:15   | 5432   | Base de datos principal            |
| Redis        | redis:7       | 6379   | Caché y comunicación entre servicios |

---

## 🔧 Configuración Inicial

### 1. Crear estructura base

```bash
mkdir microservices-lab
cd microservices-lab
mkdir auth-service blog-service email-service frontend reverse-proxy
2. Inicializar Git y conectar con GitHub
bash
Copiar código
git init
git branch -M main
git add .
git commit -m "Estructura inicial del laboratorio de microservicios"

# Reemplaza <tu-org> con tu usuario/organización
git remote add origin https://github.com/<tu-org>/microservices-lab.git
git push -u origin main
3. Archivo docker-compose.yml
Crea este archivo en la raíz microservices-lab/docker-compose.yml:

yaml
Copiar código
version: "3.9"
services:
  postgres:
    image: postgres:15
    container_name: db_postgres
    restart: always
    environment:
      POSTGRES_USER: devuser
      POSTGRES_PASSWORD: devpass
      POSTGRES_DB: main_db
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

  redis:
    image: redis:7
    container_name: cache_redis
    restart: always
    ports:
      - "6379:6379"

volumes:
  pgdata:
4. Levantar los contenedores
bash
Copiar código
docker compose up -d
docker ps
```
 ### Si ves db_postgres y cache_redis activos, el entorno está listo.

### Variables de Entorno
Crea el archivo .env.example en la raíz:
```bash
ini
Copiar código
POSTGRES_USER=devuser
POSTGRES_PASSWORD=devpass
POSTGRES_DB=main_db
REDIS_HOST=redis
REDIS_PORT=6379
```
### Copia este archivo como .env para tu entorno local y NO lo subas al repositorio.

### Mini-Reto del Día
 Levantar los contenedores con docker compose up -d
 Crear en auth-service/ un archivo test_connection.py que pruebe conexión con PostgreSQL y Redis
 Ejecutarlo dentro del contenedor con:

```bash
Copiar código
docker exec -it <nombre_contenedor> python test_connection.py
```
### Entregables del Día 1
Entregable	Descripción
Repositorio GitHub	Subido con estructura base y .env.example
Docker Compose	Levanta PostgreSQL y Redis sin errores
README Documentado	Incluye arquitectura, servicios y pasos
Evidencia visual	Captura o video mostrando docker ps con contenedores activos

### final