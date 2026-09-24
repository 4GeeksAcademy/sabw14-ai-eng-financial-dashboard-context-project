# Regla: validar comportamiento con pruebas antes de cambiar contratos o cálculos

## Alcance

- `backend/tests/**`
- `frontend/src/lib/**`
- `backend/app/**`
- `frontend/src/**`

## Justificación

La base del proyecto incluye pruebas explícitas para validar comportamiento clave:

- El backend valida endpoints, filtros y estructura de respuesta en [backend/tests/test_routes.py](../../backend/tests/test_routes.py).
- El frontend valida cálculos financieros y formateo en [frontend/src/lib/financial-utils.test.ts](../../frontend/src/lib/financial-utils.test.ts).
- El valor del contrato de datos aparece en tipos y utilidades en [frontend/src/lib/financial-types.ts](../../frontend/src/lib/financial-types.ts) y [frontend/src/lib/financial-utils.ts](../../frontend/src/lib/financial-utils.ts).

La presencia de estas pruebas indica que los cambios no deben hacerse a ciegas ni sin validar el comportamiento esperable.

## Guía concreta

- Antes de tocar endpoint, payload o cálculo, ejecutar primero la prueba relevante.
- Si se agrega un nuevo cálculo o cambio de regla financiera, duplicar el caso de prueba en el archivo correspondiente.
- Si un cambio modifica el contrato de respuesta, actualizar todas las pruebas afectadas y confirmar que el frontend sigue consumiendo la estructura esperada.
- No asumir que la UI “funciona” solo porque se ve bien; la validación real está en tests y en la API.
