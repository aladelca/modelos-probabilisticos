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

## Ejercicio 4

Datos:

- $P(D)=0.02$.
- $P(+\mid D)=0.95$.
- $P(-\mid D^c)=0.90$, entonces $P(+\mid D^c)=0.10$.

Likelihood ratio positivo:

$$
LR_+=\frac{0.95}{0.10}=9.5
$$

Posterior con un positivo:

$$
P(D\mid +)=
\frac{0.95(0.02)}{0.95(0.02)+0.10(0.98)}
=0.1624
$$

Actualización secuencial:

$$
P(D\mid +,+)=
\frac{0.95P(D\mid +)}{0.95P(D\mid +)+0.10(1-P(D\mid +))}
=0.6480
$$

Forma de odds:

$$
\text{odds prior}=\frac{0.02}{0.98}=0.0204
$$

$$
\text{odds posterior}=\frac{0.02}{0.98}(9.5)^2=1.8418
$$

$$
P(D\mid +,+)=\frac{1.8418}{1+1.8418}=0.6480
$$

El supuesto crítico es independencia condicional entre pruebas dado $D$ y dado $D^c$. Si ambas pruebas usan la misma muestra, el mismo sensor o el mismo sesgo de medición, multiplicar por $LR_+$ dos veces sobreestima la evidencia.

## Ejercicio 5

Una solución correcta debe seguir este procedimiento.

Primero, verificar que cada columna de la matriz $P(S\mid\theta)$ sume 1:

$$
\sum_s P(S=s\mid\theta)=1
$$

Luego calcular la marginal de cada señal:

$$
P(S=s)=\sum_\theta P(S=s\mid\theta)P(\theta)
$$

Después actualizar estados con Bayes:

$$
P(\theta\mid S=s)=
\frac{P(S=s\mid\theta)P(\theta)}{P(S=s)}
$$

Para cada señal, elegir la mejor acción:

$$
a^*(s)=\arg\max_a \sum_\theta U(a,\theta)P(\theta\mid S=s)
$$

El valor esperado con señal es:

$$
EV_{\text{con señal}}=
\sum_s P(S=s)\max_a \sum_\theta U(a,\theta)P(\theta\mid S=s)
$$

El valor de información imperfecta es:

$$
EVSI=EV_{\text{con señal}}-\max_a \sum_\theta U(a,\theta)P(\theta)
$$

Debe verificarse:

$$
0\le EVSI\le EVPI
$$

El valor neto es:

$$
EVSI-\text{costo de la señal}
$$

Si el valor neto es negativo, la señal puede ser estadísticamente informativa pero económicamente no conveniente.
