# Regla: separar tipos, cálculo y vista en frontend

## Alcance

- `frontend/src/**`
- `frontend/src/lib/**`
- `frontend/src/components/**`

## Justificación

El frontend está organizado por capas:

- Los tipos están en [frontend/src/lib/financial-types.ts](../../frontend/src/lib/financial-types.ts).
- La lógica de cálculo financiera vive en [frontend/src/lib/financial-utils.ts](../../frontend/src/lib/financial-utils.ts).
- La vista principal consume esos resultados en [frontend/src/App.tsx](../../frontend/src/App.tsx).
- Las pruebas de reglas de negocio están en [frontend/src/lib/financial-utils.test.ts](../../frontend/src/lib/financial-utils.test.ts).

La estructura sugiere que la vista no debe incorporar lógica de dominio compleja ni redefinir tipos improvisados.

## Guía concreta

- Mantener los tipos en `frontend/src/lib/financial-types.ts`.
- Mantener cálculo de KPI y agregados en `frontend/src/lib/financial-utils.ts`.
- Mantener los componentes visuales en `frontend/src/components/**` y evitar lógica de negocio dentro de JSX.
- Si se agrega nueva lógica, añadir prueba equivalente en [frontend/src/lib/financial-utils.test.ts](../../frontend/src/lib/financial-utils.test.ts).
