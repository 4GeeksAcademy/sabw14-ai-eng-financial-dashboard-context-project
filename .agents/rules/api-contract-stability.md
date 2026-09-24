# Regla: estabilidad del contrato API

## Alcance

- `backend/app/**`
- `frontend/src/**`
- `backend/tests/**`

## Justificación

El contrato entre frontend y backend está explícitamente definido por modelos y respuestas concretas:

- El backend define modelos de datos en [backend/app/routes.py](../../backend/app/routes.py).
- El frontend usa esos mismos tipos en [frontend/src/lib/financial-types.ts](../../frontend/src/lib/financial-types.ts).
- La UI consume la respuesta real en [frontend/src/App.tsx](../../frontend/src/App.tsx).
- Las pruebas validan ese comportamiento en [backend/tests/test_routes.py](../../backend/tests/test_routes.py).

Esto muestra que cambios en los nombres o estructura de los campos pueden romper tanto la vista como las pruebas.

## Guía concreta

- No renombrar campos como `create_date`, `operation_type`, `category` o `business_type` sin revisar todas sus referencias.
- Mantener los tipos de respuesta consistentes con la UI y con los tests.
- Si se altera un endpoint, verificar también el consumo del frontend y las pruebas del backend.
- Preferir cambios compatibles y de migración gradual sobre cambios breaking que rompan el contrato previo.
