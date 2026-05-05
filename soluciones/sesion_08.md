# Soluciones - Sesión 08

## Ejercicio 1

Para tiempos lognormales, normalmente Lognormal o Gamma ajustan mejor que Exponencial porque la Exponencial impone tasa de riesgo constante y menor flexibilidad de forma.

La probabilidad de riesgo se calcula como:

$$
P(X>c)=1-F(c)
$$

## Ejercicio 2

Al aumentar $n$, la distribución de $\bar{X}$ se aproxima a:

$$
\bar{X}\approx N\left(\mu,\frac{\sigma^2}{n}\right)
$$

Aunque la población sea asimétrica, los promedios se vuelven más simétricos y menos dispersos.

## Ejercicio 3

Lectura esperada:

- `squared_error`: buen RMSE, estima media condicional.
- `quantile`: útil para mediana o percentiles operativos.
- `poisson`: coherente con conteos, pero puede fallar con sobredispersión.
- `gamma`: útil si se trata demanda como positiva continua asimétrica.

Para inventario, cuantiles suelen ser más accionables que la media porque protegen contra faltantes de stock.
