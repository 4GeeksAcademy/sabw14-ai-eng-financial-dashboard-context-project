# Findings y reglas propuestas

Este documento recoge las reglas de trabajo que parecen estar implícitas en el repositorio y el hecho concreto del proyecto que las justifica.

## 1) Regla: mantener frontend y backend como servicios separados

**Regla propuesta:**
El proyecto debe mantener una separación clara entre la capa de presentación y la capa de API, y orquestarlas con Docker Compose.

**Hecho del repo que la justifica:**
El archivo [docker-compose.yml](../docker-compose.yml) define dos servicios independientes: `frontend` y `backend`, cada uno con su propio `build` y `ports`. Además, el frontend declara `depends_on: - backend`, lo que deja claro que existe una dependencia de servicio pero no una fusión de capas.

## 2) Regla: el backend debe exponerse como una API FastAPI con un punto de entrada único

**Regla propuesta:**
La API debe arrancar desde un único punto de entrada (`app.main:app`) y no mezclarse con lógica de UI ni de infraestructura.

**Hecho del repo que la justifica:**
En [backend/app/main.py](../backend/app/main.py), la app se crea con `FastAPI` y se monta con `app.include_router(router)`. El arranque real se especifica en [backend/Dockerfile](../backend/Dockerfile), donde se ejecuta `uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload`.

## 3) Regla: el frontend debe usar una capa de tipos y una capa de cálculo separada de la vista

**Regla propuesta:**
La capa de presentación no debe mezclar tipos de dominio, lógica de negocio y renderizado. Debe existir una separación entre tipos, utilidades y componentes.

**Hecho del repo que la justifica:**
Los tipos están en [frontend/src/lib/financial-types.ts](../frontend/src/lib/financial-types.ts), la lógica de cálculo en [frontend/src/lib/financial-utils.ts](../frontend/src/lib/financial-utils.ts), y la vista principal en [frontend/src/App.tsx](../frontend/src/App.tsx). El componente consume los datos transformados por funciones dedicadas en lugar de calcular directamente todo en el JSX.

## 4) Regla: el frontend debe consumir la API a través de una ruta estable `/api`

**Regla propuesta:**
El cliente debe apuntar a endpoints del backend mediante un prefijo estable y no hardcodear URLs internas del contenedor.

**Hecho del repo que la justifica:**
En [frontend/src/App.tsx](../frontend/src/App.tsx), la app hace `fetch(`${API_BASE_URL}/api/metrics`)`. La configuración del proxy de Vite está en [frontend/vite.config.ts](../frontend/vite.config.ts), donde `/api` redirige a `http://backend:8000`.

## 5) Regla:todo cambio de contrato de datos debe actualizar tanto backend como frontend

**Regla propuesta:**
Los cambios en la estructura del payload o en los nombres de campos deben coordinarse entre la API y la UI; no se deben romper contratos silenciosamente.

**Hecho del repo que la justifica:**
El backend define los modelos y respuestas en [backend/app/routes.py](../backend/app/routes.py), mientras el frontend consume esos datos usando tipos explícitos en [frontend/src/lib/financial-types.ts](../frontend/src/lib/financial-types.ts) y la lógica de consumo en [frontend/src/App.tsx](../frontend/src/App.tsx). Además, las pruebas de backend en [backend/tests/test_routes.py](../backend/tests/test_routes.py) y del frontend en [frontend/src/lib/financial-utils.test.ts](../frontend/src/lib/financial-utils.test.ts) validan ese contrato.

## 6) Regla: las pruebas son una garantía del comportamiento requerido

**Regla propuesta:**
Cualquier cambio funcional debe validarse con pruebas y no solo “parecer correcto” a simple vista.

**Hecho del repo que la justifica:**
El backend incluye pruebas de endpoint y de filtros en [backend/tests/test_routes.py](../backend/tests/test_routes.py), y el frontend incluye pruebas unitarias para métricas y formateo en [frontend/src/lib/financial-utils.test.ts](../frontend/src/lib/financial-utils.test.ts). La existencia de estas pruebas sugiere que el comportamiento esperado está formalizado.

## 7) Regla: las fechas deben mantenerse en ISO y orden cronológico

**Regla propuesta:**
Las fechas deben viajar en formato ISO y mantenerse ordenadas para asegurar filtros y agregados correctos.

**Hecho del repo que la justifica:**
En [frontend/src/lib/financial-types.ts](../frontend/src/lib/financial-types.ts), `create_date` se define como `string // ISO date`. En [backend/app/routes.py](../backend/app/routes.py), la lógica de ordenamiento usa `ensure_chronological_order` y los cálculos agrupan por fecha. Esto aparece claramente en la generación, filtrado y resumen de movimientos.

## 8) Regla: los puertos y la red local deben permanecer estables en desarrollo

**Regla propuesta:**
El stack local debe seguir usando los puertos documentados y mantener el proxy interno consistente para que el entorno de desarrollo no se rompa.

**Hecho del repo que la justifica:**
Se documentan los puertos en [docker-compose.yml](../docker-compose.yml), [backend/Dockerfile](../backend/Dockerfile), [frontend/Dockerfile](../frontend/Dockerfile) y [README.md](../README.md). El frontend usa 5173 y el backend 8000; el proxy de Vite redirige a `backend:8000` en [frontend/vite.config.ts](../frontend/vite.config.ts).

## 9) Regla: los datos financieros deben tratarse con tipos explícitos y valores numéricos bien definidos

**Regla propuesta:**
Los movimientos financieros deben tener una enumeración explícita de operación, categoría y tipo de negocio; no deben manejarse como strings libres o valores anárquicos.

**Hecho del repo que la justifica:**
En [backend/app/routes.py](../backend/app/routes.py), las clases `FinancialMovement`, `MetricsFacets`, `MetricsSummaryItem` y los `Literal` definen exactamente los valores posibles: `income`, `outcome`, `suppliers`, `sales`, `B2B`, etc. En el frontend, [frontend/src/lib/financial-types.ts](../frontend/src/lib/financial-types.ts) repite esa misma tipificación para mantener contract compatibility.

## 10) Regla: el proyecto debe mantenerse legible y localmente ejecutable con pocos pasos

**Regla propuesta:**
La forma de levantar el proyecto debe ser simple, reproducible y documentada para que un agente o un desarrollador la siga sin ambigüedad.

**Hecho del repo que la justifica:**
El comando de arranque aparece en [README.md](../README.md): `docker compose up --build`. Allí también se documentan las URLs esperadas del frontend, backend y Swagger. Esta documentación hace explícito el flujo de ejecución local del repositorio.

## Conclusión

Las reglas anteriores no son arbitrarias: surgen de patrones repetidos en la estructura del proyecto. El repositorio define claramente:

- un backend FastAPI con modelos y endpoints,
- un frontend React con tipos y utilidades separadas,
- un proxy de Vite para `/api`,
- una composición en Docker para orquestar ambos servicios,
- y tests que validan comportamiento y contratos.

Un agente futuro que ignore estas convenciones corre riesgo de romper la integración entre frontend y backend, desalinear tipos y payloads, o corromper el flujo de arranque local.
