# Product Overview

## Que es

El proyecto es un dashboard de metricas financieras para una vista ejecutiva de ingresos, egresos y rentabilidad. La pantalla principal carga movimientos desde `/api/metrics`, calcula los agregados en el frontend y los presenta en una vista unica.

## KPIs

La fila de indicadores (`KPIRow`) muestra cuatro KPIs:

- **Total Income**: suma de los movimientos con tipo `income`.
- **Total Outcome**: suma de los movimientos con tipo `outcome`.
- **Profit**: ingresos menos egresos.
- **Profit Margin**: beneficio como porcentaje de los ingresos.

Durante la carga, las tarjetas muestran skeletons. Si la API falla, la pantalla muestra un mensaje de error.

## Graficos

La pantalla incluye dos graficos mensuales:

- **Income vs. Outcome**: grafico de lineas con la evolucion mensual de ingresos y egresos, con tooltip, leyenda y valores monetarios.
- **Profit Margin %**: grafico de lineas con el margen de beneficio mensual y una linea de referencia en cero.

Ambos graficos muestran un estado vacio cuando no hay datos y se alimentan de los agregados calculados por `computeMonthlyData`.

## Componentes principales

La composicion actual de la vista es:

- `DashboardHeader`: titulo del dashboard y periodo mostrado.
- `KPIRow` y `KPICard`: indicadores resumidos.
- `IncomeOutcomeChart`: ingresos frente a egresos.
- `ProfitPercentChart`: margen de beneficio mensual.
