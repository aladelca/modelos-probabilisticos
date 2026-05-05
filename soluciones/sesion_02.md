# Soluciones - Sesión 02

## Ejercicio 1

Datos:

- $P(D)=0.03$.
- $P(+\mid D)=0.92$.
- $P(-\mid D^c)=0.88$, entonces $P(+\mid D^c)=0.12$.

Probabilidad total:

$$
P(+)=0.92(0.03)+0.12(0.97)=0.144
$$

Posterior:

$$
P(D\mid +)=\frac{0.92(0.03)}{0.144}=0.1917
$$

Interpretación: aunque la prueba sea razonablemente buena, un positivo implica alrededor de 19% de probabilidad de enfermedad porque la prevalencia es baja.

## Ejercicio 2

La solución depende de la matriz de pagos propuesta. El procedimiento correcto es:

1. Para cada acción $a$, calcular $E[U(a,\theta)]=\sum_\theta U(a,\theta)P(\theta)$.
2. Elegir $\max_a E[U(a,\theta)]$.
3. Con información perfecta, calcular $\sum_\theta P(\theta)\max_a U(a,\theta)$.
4. $EVPI=Valor\ con\ información\ perfecta-Mejor\ valor\ sin\ información$.

## Ejercicio 3

La implementación esperada debe:

- Evitar usar `alive` como predictor porque replica `survived`.
- Discretizar `age` y `fare`.
- Estimar priors $P(Y)$.
- Estimar tablas condicionales con:

$$
\hat{P}(X_j=x\mid Y=y)=\frac{n_{jxy}+\alpha}{n_y+\alpha K_j}
$$

El notebook 02 contiene una solución completa y comparación contra `CategoricalNB`.
