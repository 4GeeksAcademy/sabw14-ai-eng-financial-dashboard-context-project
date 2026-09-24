# Frontend

## Propósito

El frontend es la capa de presentación del dashboard financiero. Está desarrollado con React + TypeScript y sirve la interfaz visual del proyecto.

## Cómo se levanta

Se levanta como un servicio de Docker definido en [docker-compose.yml](../../docker-compose.yml) y se ejecuta con Vite desde [frontend/Dockerfile](../../frontend/Dockerfile):

```bash
npm run dev -- --host 0.0.0.0 --port 5173
```

## Puertos

- `5173` para la aplicación web

## Configuración relevante

- [frontend/Dockerfile](../../frontend/Dockerfile)
- [frontend/vite.config.ts](../../frontend/vite.config.ts)
- [frontend/package.json](../../frontend/package.json)

## Comunicación con backend

El frontend usa un proxy para redirigir peticiones a `/api` hacia el backend:

```ts
proxy: {
  "/api": {
    target: "http://backend:8000",
    changeOrigin: true,
  },
}
```

La configuración aparece en [frontend/vite.config.ts](../../frontend/vite.config.ts).

## Punto de entrada

La app React se prepara en el contexto de Vite y se expone con el host `0.0.0.0` para funcionar dentro del contenedor y ser accesible desde el host.
