# Arquitectura y contexto del proyecto

## 1. Descripción general

Este repositorio contiene una aplicación web de dashboard financiero con dos componentes principales:

- Frontend: React + TypeScript + Vite
- Backend: FastAPI (Python)

La idea general es que el frontend muestre métricas financieras y el backend exponga una API REST para servir esos datos.

La documentación de uso del proyecto y los endpoints de acceso se encuentran en [README.md](../README.md).

## 2. Estructura de servicios

### Frontend

El frontend se define en [docker-compose.yml](../docker-compose.yml) como el servicio `frontend`.

Características:

- Se construye desde el directorio [frontend](../frontend)
- Usa la imagen base Node definida en [frontend/Dockerfile](../frontend/Dockerfile)
- Expone el puerto `5173`
- Depende del servicio `backend`

Configuración relevante:

- [docker-compose.yml](../docker-compose.yml)
- [frontend/Dockerfile](../frontend/Dockerfile)
- [frontend/vite.config.ts](../frontend/vite.config.ts)

### Backend

El backend se define en [docker-compose.yml](../docker-compose.yml) como el servicio `backend`.

Características:

- Se construye desde el directorio [backend](../backend)
- Usa la imagen base Python definida en [backend/Dockerfile](../backend/Dockerfile)
- Expone los puertos `8000` y `5678`
- El puerto `8000` es el de la API, y `5678` está dedicado a debugpy / depuración remota

Configuración relevante:

- [docker-compose.yml](../docker-compose.yml)
- [backend/Dockerfile](../backend/Dockerfile)

## 3. Entry points y arranque

### Backend entry point

El backend se inicia con FastAPI desde [backend/app/main.py](../backend/app/main.py).

Ese archivo crea la aplicación:

- `app = FastAPI(title="Financial Metrics API")`
- `app.include_router(router)`

Esto indica que el punto de entrada principal es `app.main:app`, y la ejecución real viene definida en [backend/Dockerfile](../backend/Dockerfile):

```bash
python -m debugpy --listen 0.0.0.0:5678 -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

Esto confirma que:

- la API corre en `0.0.0.0:8000`
- el debug remoto escucha en `0.0.0.0:5678`

### Frontend entry point

El frontend se inicia con Vite desde [frontend/Dockerfile](../frontend/Dockerfile):

```bash
npm run dev -- --host 0.0.0.0 --port 5173
```

Además, la configuración del servidor de desarrollo en [frontend/vite.config.ts](../frontend/vite.config.ts) especifica:

- `host: "0.0.0.0"`
- proxy `/api` hacia `http://backend:8000`

Esto hace que el navegador del usuario acceda al frontend en el puerto `5173`, mientras las peticiones a `/api` son re-enviadas al backend del servicio Docker llamado `backend`.

## 4. Puertos utilizados

Se observan los siguientes puertos:

| Servicio | Puerto | Uso | Fuente |
|---|---:|---|---|
| Frontend | 5173 | UI del dashboard | [docker-compose.yml](../docker-compose.yml), [frontend/Dockerfile](../frontend/Dockerfile) |
| Backend API | 8000 | API FastAPI | [docker-compose.yml](../docker-compose.yml), [backend/Dockerfile](../backend/Dockerfile) |
| Backend debug | 5678 | Depuración remota con debugpy | [docker-compose.yml](../docker-compose.yml), [backend/Dockerfile](../backend/Dockerfile) |

## 5. Cómo se levanta el proyecto

La forma recomendada de levantar todo el stack es la siguiente, según [README.md](../README.md):

```bash
docker compose up --build
```

La documentación del README además indica las URLs esperadas:

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:8000`
- API docs: `http://localhost:8000/docs`

## 6. Contexto funcional del proyecto

El nombre del proyecto y su contenido sugieren un dashboard financiero orientado a métricas de negocio, con un frontend visual y un backend que expone datos financieros a través de una API.

El repo se organiza de forma mínima y clara:

- [backend](../backend): lógica del servidor y rutas
- [frontend](../frontend): interfaz y visualización de métricas
- [docker-compose.yml](../docker-compose.yml): orquestación de servicios
- [README.md](../README.md): instrucciones y contexto de arranque

## 7. Resumen ejecutivo

La arquitectura del proyecto es una aplicación full-stack simple:

- El frontend está servido por Vite en el puerto `5173`
- El backend está hecho con FastAPI y corre en el puerto `8000`
- El frontend usa un proxy `/api` para conectar con el backend
- La orquestación entre ambos servicios la maneja Docker Compose
- La forma de arrancar el proyecto es `docker compose up --build`

Este conjunto de decisiones refleja una estructura de desarrollo directa, fácil de ejecutar y adecuada para un ejercicio o proyecto académico de dashboard financiero con backend y frontend desacoplados.
