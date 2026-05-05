# Soluciones - Sesión 05

## Ejercicio 1

Con lognormal se espera media mayor que mediana y asimetría positiva. Al agregar outliers:

- La media sube mucho.
- La desviación estándar sube mucho.
- La mediana cambia poco.
- El IQR cambia poco si los outliers están fuera del centro.

Esto muestra robustez de mediana e IQR frente a colas.

## Ejercicio 2

Para $X\sim Binomial(6,0.35)$:

$$
E[X]=np=2.1
$$

$$
Var(X)=np(1-p)=6(0.35)(0.65)=1.365
$$

Desde la PMF:

$$
E[X^2]=Var(X)+E[X]^2=1.365+2.1^2=5.775
$$

## Ejercicio 3

Se debe construir una tabla:

$$
E[Demand\mid Promotion]
$$

y otra:

$$
E[Demand\mid Promotion,Weather]
$$

Si las medias condicionales cambian de forma consistente, hay evidencia descriptiva de que esos factores segmentan la distribución. No implica causalidad sin controlar diseño o confusores.
