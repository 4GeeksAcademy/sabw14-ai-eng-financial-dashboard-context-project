# Backend

## Propósito

El backend es la capa de API del proyecto. Está implementado con FastAPI y expone los datos financieros que el frontend consume.

## Cómo se levanta

Se despliega como servicio Docker definido en [docker-compose.yml](../../docker-compose.yml) y se inicia con Uvicorn desde [backend/Dockerfile](../../backend/Dockerfile):

```bash
python -m debugpy --listen 0.0.0.0:5678 -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

## Puertos

- `8000` para la API
- `5678` para depuración con debugpy

## Punto de entrada

La app se inicia en [backend/app/main.py](../../backend/app/main.py):

```python
app = FastAPI(title="Financial Metrics API")
app.include_router(router)
```

Esto indica que el punto principal de arrancada es `app.main:app`.

## Configuración relevante

- [backend/app/main.py](../../backend/app/main.py)
- [backend/Dockerfile](../../backend/Dockerfile)
- [backend/app/routes.py](../../backend/app/routes.py)

## Documentación API

La documentación interactiva de FastAPI se sirve en:

- `http://localhost:8000/docs`

Según se indica en [README.md](../../README.md).
