# Verification

| Afirmación del agente | Estado | Evidencia / corrección |
|---|---|---|
| El repo tiene dos servicios principales: frontend y backend | ✅ | `docker-compose.yml` define `frontend` y `backend` como servicios separados. |
| El frontend usa React + TypeScript + Vite | ✅ | `frontend/package.json` declara `react`, `react-dom`, `vite`, y `@vitejs/plugin-react`; además `frontend/vite.config.ts` configura Vite. |
| El backend usa FastAPI | ✅ | `backend/app/main.py` crea `app = FastAPI(title="Financial Metrics API")`. |
| El backend corre en el puerto 8000 | ✅ | `backend/Dockerfile` ejecuta `uvicorn ... --port 8000`; `docker-compose.yml` mapea `"8000:8000"`. |
| El frontend corre en el puerto 5173 | ✅ | `frontend/Dockerfile` ejecuta Vite con `--port 5173`; `docker-compose.yml` mapea `"5173:5173"`. |
| El backend tiene depuración remota en el puerto 5678 | ✅ | `backend/Dockerfile` ejecuta `python -m debugpy --listen 0.0.0.0:5678`; `docker-compose.yml` mapea `"5678:5678"`. |
| El frontend se comunica con el backend mediante proxy `/api` | ✅ | `frontend/vite.config.ts` define `proxy: { "/api": { target: "http://backend:8000" } }`. |
| El proyecto se levanta con `docker compose up --build` | ✅ | `README.md` documenta ese comando como la forma recomendada de arranque. |
| El frontend está disponible en `http://localhost:5173` | ✅ | `README.md` indica "Frontend: http://localhost:5173". |
| El backend está disponible en `http://localhost:8000` | ✅ | `README.md` indica "Backend: http://localhost:8000". |
| La API expone Swagger / Docs en `/docs` | ✅ | `README.md` indica "API documentation: http://localhost:8000/docs". |
| El backend es el entry point de la app | ✅ | `backend/app/main.py` incluye `app.include_router(router)` y el contenedor lo ejecuta con `uvicorn app.main:app`. |
| El proyecto necesita un archivo `.env` para funcionar localmente | ❓ | La documentación dice que no es necesario por defecto; solo si se quiere apuntar a otro backend, se puede crear `frontend/.env.example` y luego `.env`. |
| La app tiene una base de datos | ❌ | No hay evidencia en el repo de PostgreSQL, MySQL u otra base de datos en `docker-compose.yml` ni en los archivos del backend. |
| El frontend está completamente desacoplado del backend | ❓ | Está desacoplado a nivel de servicios, pero sí se comunica a través del proxy `/api` definido en `vite.config.ts`. |
