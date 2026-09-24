# Regla: conservar la topología de red y puertos del entorno local

## Alcance

- `docker-compose.yml`
- `backend/Dockerfile`
- `frontend/Dockerfile`
- `frontend/vite.config.ts`

## Justificación

El entorno local está definido en varios archivos y se apoya en puertos y proxy estables:

- La composición de servicios está en [docker-compose.yml](../../docker-compose.yml).
- El backend expone `8000` y `5678` en [backend/Dockerfile](../../backend/Dockerfile).
- El frontend expone `5173` en [frontend/Dockerfile](../../frontend/Dockerfile).
- El proxy del frontend redirige `/api` hacia `http://backend:8000` en [frontend/vite.config.ts](../../frontend/vite.config.ts).

Esto evidencia que el entorno de desarrollo depende de una topología de red explícita.

## Guía concreta

- No cambiar puertos `5173` y `8000` sin actualizar `docker-compose.yml`, Dockerfiles y la configuración de Vite.
- Mantener el nombre del servicio `backend` para que el proxy siga funcionando.
- Si se cambia la API, verificar también la redirección `/api` declarada en [frontend/vite.config.ts](../../frontend/vite.config.ts).
- Usar los puertos documentados en [README.md](../../README.md) para evitar inconsistencias en local development.
