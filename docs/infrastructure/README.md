# Infraestructura y despliegue

## Orquestación

El proyecto se levanta usando Docker Compose, definido en [docker-compose.yml](../../docker-compose.yml).

Ese archivo declara dos servicios:

- `frontend`
- `backend`

## Servicios principales

### Frontend

- Contexto: [frontend](../../frontend)
- Puerto: `5173`
- Dependencia: `backend`

### Backend

- Contexto: [backend](../../backend)
- Puertos: `8000`, `5678`
- Tipo: API FastAPI con depuración remota

## Comando de arranque

Según [README.md](../../README.md), el proyecto se levanta con:

```bash
docker compose up --build
```

## URLs esperadas

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:8000`
- Documentación API: `http://localhost:8000/docs`

## Archivos clave

- [docker-compose.yml](../../docker-compose.yml)
- [README.md](../../README.md)
- [frontend/Dockerfile](../../frontend/Dockerfile)
- [backend/Dockerfile](../../backend/Dockerfile)
- [frontend/vite.config.ts](../../frontend/vite.config.ts)
- [backend/app/main.py](../../backend/app/main.py)
