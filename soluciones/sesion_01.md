# Soluciones - Sesión 01

## Ejercicio 1

Hay 36 resultados equiprobables.

- $P(A)=18/36=0.5$.
- $P(B)=18/36=0.5$.
- $P(C)=6/36=1/6$ porque las sumas 10, 11 y 12 tienen 3, 2 y 1 casos.
- $P(A\cap B)=9/36=0.25$.
- $P(A\cup C)=P(A)+P(C)-P(A\cap C)$. Como $A\cap C$ corresponde a suma 10 o 12, tiene $3+1=4$ casos. Entonces $P(A\cup C)=18/36+6/36-4/36=20/36$.

Independencia: $P(A\cap B)=0.25=P(A)P(B)$, por tanto $A$ y $B$ son independientes.

## Ejercicio 2

En $\{0,\ldots,9\}$:

- $D=\{0,2,4,6,8\}$.
- $E=\{5,6,7,8,9\}$.
- $F=\{0,3,6,9\}$.

$P(D\cup E)=|\{0,2,4,5,6,7,8,9\}|/10=0.8$.

$P(D\cap F)=|\{0,6\}|/10=0.2$.

$P(D\cap E)=|\{6,8\}|/10=0.2$, pero $P(D)P(E)=0.5\times0.5=0.25$. No son independientes.

## Ejercicio 3

La frecuencia relativa converge hacia $p$ al crecer $n$, pero no coincide exactamente por variabilidad muestral. La desviación típica de la frecuencia es:

$$
\sqrt{\frac{p(1-p)}{n}}
$$

Por eso el error esperado baja como $1/\sqrt{n}$.
