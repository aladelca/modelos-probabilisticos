# Soluciones - Sesión 06

## Ejercicio 1

La verificación debe mostrar:

$$
|Cov(X,Y)|\le\sigma_X\sigma_Y
$$

Si se divide ambos lados por $\sigma_X\sigma_Y$, se obtiene:

$$
|\rho_{XY}|\le1
$$

## Ejercicio 2

En `sales_data.csv` típicamente aparece una correlación alta entre `Demand` y `Units Sold`, y entre `Price` y `Competitor Pricing`.

Advertencia: correlación alta no implica causalidad. `Units Sold` puede estar mecánicamente ligada a `Demand`; `Price` y `Competitor Pricing` pueden compartir reglas de generación.

## Ejercicio 3

La serie diaria debe ordenarse por `Date`. Para AR(1):

$$
Demand_t=c+\phi Demand_{t-1}+\varepsilon_t
$$

El estimador por regresión con rezago entrega una lectura de persistencia. Si $\phi$ está cerca de cero, hay poca memoria lineal de primer orden; si está cerca de 1, hay alta persistencia.
