# Soluciones - Sesión 03

## Ejercicio 1

Las tres variables son discretas:

- $X$ toma valores de 2 a 12.
- $Y$ toma valores de 1 a 6.
- $Z$ toma valores 0 y 1.

Para construir la PMF, se enumeran los 36 pares posibles y se cuenta la frecuencia relativa de cada valor.

## Ejercicio 2

Clasificación típica:

- Categóricas: `Store ID`, `Product ID`, `Category`, `Region`, `Weather Condition`, `Seasonality`.
- Bernoulli: `Promotion`, `Epidemic`.
- Conteos: `Inventory Level`, `Units Sold`, `Units Ordered`, `Demand`.
- Continuas positivas: `Price`, `Competitor Pricing`.
- Índice temporal: `Date`.

Variables aleatorias posibles: demanda diaria, ventas por tienda, indicador de demanda alta.

Al convertir `Demand` en indicador se pierde magnitud: no se distingue entre demanda apenas alta y demanda extrema.

## Ejercicio 3

Si $X$ es lognormal, entonces $\log X$ es aproximadamente normal por construcción. La transformación reduce asimetría y hace más plausible un modelo Gaussiano para errores o features transformadas.
