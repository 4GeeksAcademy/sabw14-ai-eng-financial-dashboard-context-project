# Tech Stack

## Frontend

Las versiones se toman de `frontend/package.json` y `frontend/Dockerfile`.

- Node.js: 24, mediante `node:24-alpine`.
- React: `^19.2.4`.
- React DOM: `^19.2.4`.
- TypeScript: `~6.0.2`.
- Vite: `^8.0.4`.
- Vitest: `^4.1.4`.
- Recharts: `^3.8.1`.
- Tailwind CSS: `^4.2.2`, integrado mediante `@tailwindcss/vite` `^4.2.2`.
- Lucide React: `^1.8.0`.
- ESLint: `^9.39.4`.

El frontend expone el puerto `5173` y Vite redirige las peticiones `/api` al servicio `backend` en el puerto `8000`.

## Backend

Las versiones de runtime y dependencias se toman de `backend/Dockerfile` y `backend/requirements.txt`.

- Python: 3.13, mediante `python:3.13-slim`.
- FastAPI: declarada sin version fija.
- Uvicorn: declarado como `uvicorn[standard]`, sin version fija.
- pytest: declarado sin version fija.
- pytest-cov: declarado sin version fija.
- httpx: declarado sin version fija.
- debugpy: declarado sin version fija.

El backend expone el puerto `8000` para la API y el puerto `5678` para debugpy. El arranque usa Uvicorn con recarga.

## Infraestructura local

`docker-compose.yml` define dos servicios separados: `frontend` y `backend`. El frontend depende del backend y ambos montan su codigo local para desarrollo.
