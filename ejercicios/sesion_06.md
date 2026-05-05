# Ejercicios - Sesión 06

## Ejercicio 1: covarianza y Schwarz

Simula dos variables normales correlacionadas.

1. Calcula matriz de covarianza.
2. Calcula correlación.
3. Verifica $|Cov(X,Y)|\le\sigma_X\sigma_Y$.

## Ejercicio 2: demanda multivariada

Usa `sales_data.csv`.

1. Calcula matriz de covarianza para `Demand`, `Units Sold`, `Price`, `Discount`, `Promotion`.
2. Interpreta las tres correlaciones más grandes en valor absoluto.
3. Explica qué correlaciones podrían ser espurias.

## Ejercicio 3: serie temporal

Para `Store ID = S001` y `Product ID = P0001`:

1. Construye una serie diaria de `Demand`.
2. Calcula autocorrelación lag 1 a 7.
3. Ajusta un AR(1) por regresión con rezago.
4. Si tienes `statsmodels`, ajusta ARIMA(1,1,1).
