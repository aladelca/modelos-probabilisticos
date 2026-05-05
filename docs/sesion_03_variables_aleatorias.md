# Sesión 03 - Variables aleatorias

## Logro de la sesión

Al finalizar la sesión, el estudiante distingue variables aleatorias discretas y continuas, y transforma resultados aleatorios en magnitudes numéricas analizables.

## Definición

Una **variable aleatoria** es una función:

$$
X:\Omega\rightarrow\mathbb{R}
$$

No es el resultado aleatorio en sí, sino una medición numérica del resultado.

Ejemplo: al lanzar dos dados, el resultado elemental es $(i,j)$, pero se pueden definir varias variables:

- $X(i,j)=i+j$: suma.
- $Y(i,j)=\max(i,j)$: máximo.
- $Z(i,j)=1$ si la suma es par y $0$ si no.

## Variable discreta

Una variable aleatoria discreta toma valores finitos o contables. Se describe con una función de masa de probabilidad:

$$
p_X(x)=P(X=x)
$$

Debe cumplir:

$$
p_X(x)\ge0,\quad \sum_x p_X(x)=1
$$

Ejemplos:

- Número de compras.
- Número de reclamos.
- Click/no click.
- Categoría predicha por un clasificador.

## Variable continua

Una variable aleatoria continua puede tomar valores en intervalos. Se describe con una función de densidad:

$$
f_X(x)\ge0,\quad \int_{-\infty}^{\infty} f_X(x)\,dx=1
$$

Para variables continuas:

$$
P(X=x)=0
$$

y las probabilidades se calculan sobre intervalos:

$$
P(a\le X\le b)=\int_a^b f_X(x)\,dx
$$

Ejemplos:

- Tiempo de espera.
- Ingreso mensual.
- Temperatura.
- Duración de una sesión.

## Función de distribución acumulada

Para cualquier variable aleatoria:

$$
F_X(x)=P(X\le x)
$$

La CDF permite comparar variables discretas y continuas con una misma herramienta.

## Transformaciones

Si $Y=g(X)$, entonces $Y$ también es una variable aleatoria. Las transformaciones aparecen en:

- Estandarización: $Z=(X-\mu)/\sigma$.
- Log-transformaciones para variables positivas.
- Indicadores binarios: $I(X>c)$.
- Agregaciones: suma, promedio, máximo.

## Actividad formativa

Escoge un caso real y define tres variables aleatorias distintas sobre el mismo experimento. Indica cuáles son discretas, cuáles continuas y qué información perderías o ganarías con cada definición.

## Profundización para nivel de maestría

### Variable aleatoria como transformación medible

Formalmente, una variable aleatoria no es simplemente "una variable con incertidumbre". Es una función medible desde el espacio probabilístico hacia los reales. Esa precisión importa porque permite inducir una distribución sobre los valores numéricos:

$$
P_X(B)=P(\{\omega\in\Omega:X(\omega)\in B\})
$$

Esta distribución inducida, también llamada pushforward, es lo que realmente analizamos cuando graficamos histogramas, calculamos CDF o ajustamos modelos paramétricos.

### Elección de la variable aleatoria

La misma realidad puede producir variables distintas. En un sistema de atención:

- $X$: número de tickets por hora.
- $T$: tiempo hasta el siguiente ticket.
- $S$: severidad del ticket.
- $I$: indicador de incumplimiento de SLA.

Cada elección responde una pregunta diferente. Una buena formulación de problema empieza por definir la variable aleatoria alineada a la decisión: conteo para capacidad, tiempo para espera, indicador para riesgo, monto para pérdida.

### Distribuciones mixtas

No todo es puramente discreto o continuo. Muchos casos reales son mixtos:

- Gasto mensual: masa en 0 para clientes que no compran y distribución continua positiva para quienes compran.
- Tiempo de espera: masa en 0 para atención inmediata y cola continua para el resto.
- Reclamos: exceso de ceros más conteo positivo.

Estos casos requieren modelos como zero-inflated, hurdle models o mezclas. Tratar una variable mixta como continua simple puede sesgar probabilidades de eventos extremos o de no ocurrencia.

### Indicadores y esperanza de eventos

Para cualquier evento $A$, se puede definir:

$$
I_A(\omega)=
\begin{cases}
1,& \omega\in A\\
0,& \omega\notin A
\end{cases}
$$

Entonces:

$$
E[I_A]=P(A)
$$

Esta identidad es poderosa: convierte problemas de probabilidad en problemas de esperanza. Aparece en estimadores de Monte Carlo, clasificación binaria, métricas de error y pruebas de hipótesis.

### CDF como objeto unificador

La CDF existe para variables discretas, continuas y mixtas:

$$
F_X(x)=P(X\le x)
$$

Propiedades:

- Es no decreciente.
- Es continua por la derecha.
- $\lim_{x\to-\infty}F_X(x)=0$.
- $\lim_{x\to\infty}F_X(x)=1$.

Los saltos de la CDF indican masa puntual. Las zonas suaves representan contribución continua.

### Cuantiles y función inversa

El cuantil de orden $q$ se define como:

$$
F_X^{-1}(q)=\inf\{x:F_X(x)\ge q\}
$$

Los cuantiles son más robustos que la media para variables asimétricas. En riesgo se usan para VaR, percentiles de SLA, niveles de inventario y umbrales operativos.

### Transformaciones y cambio de variable

Si $Y=g(X)$, la distribución de $Y$ depende de la transformación. Para transformaciones monótonas y diferenciables:

$$
f_Y(y)=f_X(g^{-1}(y))\left|\frac{d}{dy}g^{-1}(y)\right|
$$

Ejemplo: si $Y=\log X$, una variable positiva con cola derecha puede volverse aproximadamente normal. Esta idea justifica transformaciones logarítmicas antes de modelar montos, precios o tiempos.

### Simulación por transformada inversa

Si $U\sim Uniforme(0,1)$, entonces:

$$
X=F^{-1}(U)
$$

tiene distribución $F$. Esta es una técnica básica para simulación y conecta la CDF con generación de datos sintéticos.

### Criterios de formulación

Para una entrega de maestría, al definir una variable aleatoria se debe especificar:

- Unidad de análisis: cliente, transacción, día, ticket, lote.
- Horizonte temporal: por minuto, día, campaña, ciclo de vida.
- Soporte: valores posibles y restricciones naturales.
- Tipo: discreta, continua, mixta, ordinal o categórica codificada.
- Transformaciones: agregaciones, indicadores, logs, winsorización.
- Decisión asociada: qué acción se tomará usando esa variable.
