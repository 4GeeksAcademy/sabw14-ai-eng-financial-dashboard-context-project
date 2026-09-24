# Regla: separación estricta entre frontend y backend

## Alcance

- `frontend/**`
- `backend/**`
- `docker-compose.yml`

## Justificación

La estructura del proyecto divide claramente la capa de presentación y la capa de API:

- El servicio `frontend` y el servicio `backend` están definidos por separado en [docker-compose.yml](../../docker-compose.yml).
- El backend se arranca como una app FastAPI en [backend/app/main.py](../../backend/app/main.py).
- El frontend se presenta como una app React/Vite en [frontend/package.json](../../frontend/package.json) y se conecta a la API mediante proxy en [frontend/vite.config.ts](../../frontend/vite.config.ts).

Esto demuestra que el repositorio considera frontend y backend como dos piezas separadas, aunque conectadas a través de la red local y un proxy.

## Guía concreta

- Mantener en `backend/**` la lógica de API, modelos y rutas.
- Mantener en `frontend/**` la lógica de UI, render y consumo de la API.
- Evitar mezclar endpoints de FastAPI con componentes React o lógica de presentacion dentro del mismo archivo.
- Si se va a cambiar la API, revisar primero el consumo del frontend en [frontend/src/App.tsx](../../frontend/src/App.tsx) y [frontend/src/lib/financial-types.ts](../../frontend/src/lib/financial-types.ts).
- Si se va a cambiar la vista, revisar el backend con cuidado para no romper el contrato de datos.
