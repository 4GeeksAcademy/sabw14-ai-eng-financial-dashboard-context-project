# Current State

## Lo que funciona

- El entorno local se levanta con Docker Compose con servicios separados para frontend y backend.
- El frontend React/Vite carga datos desde `/api/metrics` a traves del proxy de Vite.
- El backend expone `/health` y endpoints de metricas con filtros por fecha, categoria y tipo de operacion.
- La pantalla calcula y muestra total de ingresos, total de egresos, beneficio y margen de beneficio.
- La pantalla calcula y muestra series mensuales para ingresos frente a egresos y para margen de beneficio.
- Existen estados de carga, estado vacio y mensaje de error de carga en la UI.
- Hay pruebas del backend para rutas y calculos, y pruebas del frontend para las utilidades financieras.

## Gaps conocidos

- **Datos mock**: el backend genera movimientos deterministas en memoria con `generate_mock_movements(seed=42)` para cada consulta; no hay datos persistidos. Tambien existe `frontend/src/lib/mock-data.ts`, pero no tiene referencias desde el frontend.
- **Sin base de datos**: no hay una base de datos ni una capa de persistencia configurada.
- **Endpoints sin usar por la UI**: el frontend solo consume `/api/metrics`. No consume `/health`, `/api/metrics/facets`, `/api/metrics/summary`, `/api/metrics/categories/top`, `/api/metrics/comparison`, `/api/metrics/alerts`, `/api/metrics/b2b` ni `/api/metrics/b2c`.
- **CORS abierto**: el backend configura `allow_origins=["*"]`.
- **Codigo muerto o sin consumidor**: `frontend/src/lib/mock-data.ts` exporta datos que no se utilizan. Ademas, varias rutas backend estan implementadas y probadas, pero aun no tienen consumidor en la UI.
- **Periodo mostrado fijo**: `DashboardHeader` recibe actualmente el texto `2024 - Full Year`, aunque los datos del backend se generan tomando como referencia la fecha actual.

## Siguientes prioridades razonables

1. Definir la fuente de datos que sustituira la generacion mock y persistir los movimientos antes de ampliar la UI.
2. Acordar que filtros y vistas forman parte del producto y conectar solo los endpoints correspondientes, empezando por los que ya estan implementados.
3. Restringir `allow_origins` a los origenes reales de cada entorno cuando exista una configuracion de despliegue.
4. Eliminar `frontend/src/lib/mock-data.ts` si no se necesita como fixture, o convertirlo explicitamente en un fixture de pruebas.
5. Hacer que el periodo del encabezado provenga del rango real de datos o de una seleccion de usuario, en lugar de mantenerlo fijo.
