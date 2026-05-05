# Soluciones - Sesión 04

## Ejercicio 1

Una PMF válida debe cumplir:

$$
p(x)\ge0,\quad \sum_x p(x)=1
$$

Luego:

$$
E[X]=\sum_x xp(x)
$$

$$
Var(X)=\sum_x (x-E[X])^2p(x)
$$

Y:

$$
P(X\ge5)=\sum_{x=5}^{8}p(x)
$$

## Ejercicio 2

Para una triangular simétrica en $[0,10]$ con máximo en 5:

$$
f(x)=
\begin{cases}
x/25,&0\le x\le5\\
(10-x)/25,&5<x\le10\\
0,&\text{otro caso}
\end{cases}
$$

El área total es 1. La probabilidad $P(2\le X\le6)$ se calcula integrando esa densidad en dos tramos. $f(5)$ es altura de densidad, no probabilidad puntual.

## Ejercicio 3

La CDF empírica se calcula como:

$$
\hat{F}_n(x)=\frac{1}{n}\sum_i I(x_i\le x)
$$

Una comparación por `Promotion` debe reportar medias, medianas y percentiles; no solo histogramas.
