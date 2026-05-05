# Sesión 06 - Momentos de dos o más variables aleatorias

## Logro de la sesión

Al finalizar la sesión, el estudiante analiza relaciones entre variables aleatorias usando momentos conjuntos, covarianza, correlación y la desigualdad de Schwarz.

## Momentos conjuntos

Para dos variables aleatorias $X$ e $Y$, un momento conjunto tiene la forma:

$$
E[X^aY^b]
$$

Estos momentos permiten describir cómo se comportan las variables en conjunto, no solo por separado.

## Covarianza

$$
Cov(X,Y)=E[(X-\mu_X)(Y-\mu_Y)]
$$

Forma equivalente:

$$
Cov(X,Y)=E[XY]-E[X]E[Y]
$$

Interpretación:

- Positiva: valores altos de $X$ tienden a aparecer con valores altos de $Y$.
- Negativa: valores altos de $X$ tienden a aparecer con valores bajos de $Y$.
- Cercana a cero: no hay asociación lineal fuerte.

## Correlación

$$
\rho_{X,Y}=\frac{Cov(X,Y)}{\sigma_X\sigma_Y}
$$

La correlación estandariza la covarianza y queda entre -1 y 1.

## Desigualdad de Cauchy-Schwarz

Para variables con segundo momento finito:

$$
|E[XY]|\le \sqrt{E[X^2]E[Y^2]}
$$

Aplicada a variables centradas:

$$
|Cov(X,Y)|\le \sigma_X\sigma_Y
$$

Esta desigualdad garantiza que la correlación siempre está en $[-1,1]$.

## Matriz de covarianza

Para un vector aleatorio $\mathbf{X}$:

$$
\Sigma=E[(\mathbf{X}-\mu)(\mathbf{X}-\mu)^T]
$$

Es base para PCA, modelos gaussianos multivariados, simulación correlacionada, gestión de riesgo y análisis de series.

## Actividad sumativa sugerida

Usa tres variables numéricas de un caso real o simulado. Calcula la matriz de covarianza, la matriz de correlación, verifica Cauchy-Schwarz para un par y explica qué implicaría esa relación para un modelo predictivo.

## Profundización para nivel de maestría

### Distribución conjunta, marginal y condicional

Para estudiar dos variables no basta conocer cada distribución marginal. La relación está en la distribución conjunta:

$$
F_{X,Y}(x,y)=P(X\le x,Y\le y)
$$

Desde la conjunta se obtienen marginales:

$$
f_X(x)=\int f_{X,Y}(x,y)\,dy
$$

y condicionales:

$$
f_{Y\mid X}(y\mid x)=\frac{f_{X,Y}(x,y)}{f_X(x)}
$$

Dos variables pueden tener las mismas marginales y dependencias muy distintas. Por eso una matriz de correlación no describe completamente la dependencia.

### Independencia vs correlación cero

Si $X$ e $Y$ son independientes:

$$
E[XY]=E[X]E[Y]
$$

por tanto $Cov(X,Y)=0$, cuando los momentos existen. El recíproco no siempre es cierto: covarianza cero no implica independencia. Un ejemplo clásico es $Y=X^2$ con $X$ simétrica alrededor de cero; puede haber correlación cero y dependencia perfecta no lineal.

En la normal multivariada sí ocurre una excepción importante: covarianza cero implica independencia para componentes gaussianos conjuntos.

### Matriz de covarianza positiva semidefinida

Toda matriz de covarianza $\Sigma$ debe ser simétrica y positiva semidefinida:

$$
a^T\Sigma a\ge 0
$$

para cualquier vector $a$. Esto se debe a:

$$
a^T\Sigma a=Var(a^T X)
$$

Si una matriz estimada no cumple esta propiedad, puede haber problemas numéricos, datos insuficientes, duplicación de variables o una matriz construida incorrectamente.

### Demostración corta de Cauchy-Schwarz

Para variables $X$ e $Y$ con segundo momento finito, considere:

$$
E[(X-tY)^2]\ge 0
$$

para todo $t$. Al expandir:

$$
E[X^2]-2tE[XY]+t^2E[Y^2]\ge0
$$

Este polinomio cuadrático en $t$ no puede tener discriminante positivo. Por tanto:

$$
4E[XY]^2-4E[X^2]E[Y^2]\le0
$$

y:

$$
|E[XY]|\le\sqrt{E[X^2]E[Y^2]}
$$

Aplicado a $X-\mu_X$ e $Y-\mu_Y$ produce el límite de la covarianza.

### Ley de covarianza total

La covarianza puede descomponerse condicionando por una variable $Z$:

$$
Cov(X,Y)=E[Cov(X,Y\mid Z)] + Cov(E[X\mid Z],E[Y\mid Z])
$$

Esta fórmula separa asociación dentro de segmentos y asociación entre segmentos. Es útil para explicar paradojas de agregación y para decidir si conviene modelar por grupos.

### Correlación, escala y causalidad

La correlación es invariante ante transformaciones lineales positivas, pero no ante transformaciones no lineales. Además:

- No mide relaciones no lineales generales.
- Puede estar dominada por outliers.
- No prueba causalidad.
- Puede aparecer por variables omitidas o tendencias compartidas.

En reportes de maestría, una correlación debe acompañarse de gráfico, contexto temporal y discusión de posibles confusores.

### Dependencia de colas

Dos variables pueden tener correlación moderada pero moverse juntas en extremos. En riesgo financiero, seguros, fraude y operaciones, la dependencia de colas puede ser más importante que la correlación promedio.

Herramientas avanzadas:

- Copulas para separar marginales y dependencia.
- Kendall tau y Spearman rho para dependencia monotónica.
- Análisis de cuantiles condicionales.
- Simulación multivariada con matrices de covarianza validadas.

### PCA como lectura de covarianza

PCA diagonaliza la matriz de covarianza o correlación. Busca direcciones $w$ que maximizan:

$$
Var(w^TX)
$$

sujeto a $\|w\|=1$. La primera componente captura máxima varianza lineal; las siguientes capturan varianza residual bajo ortogonalidad. PCA no maximiza poder predictivo supervisado: resume estructura no supervisada.

### Momentos en series de tiempo

En series temporales, la dependencia se expresa con autocovarianza:

$$
\gamma(k)=Cov(X_t,X_{t-k})
$$

Si la serie es débilmente estacionaria, la media es constante y $\gamma(k)$ depende solo del rezago $k$, no del tiempo $t$. Esta idea sostiene ARMA/ARIMA, modelos de volatilidad y diagnóstico de residuales.

### Requisitos para análisis multivariado

Un informe sólido debe incluir:

- Matriz de covarianza y correlación con interpretación.
- Verificación de signos, magnitudes y escalas.
- Discusión de independencia vs asociación lineal.
- Gráficos de pares relevantes.
- Revisión de outliers y segmentos.
- Validación de Cauchy-Schwarz para mostrar coherencia de momentos.
- Si hay tiempo, revisión de autocorrelación.

## Series de tiempo: de autocovarianza a ARIMA, ARIMAX y VAR

La autocovarianza es un momento conjunto entre una variable y su propio pasado:

$$
\gamma(k)=Cov(X_t,X_{t-k})
$$

La autocorrelación normaliza ese momento:

$$
\rho(k)=\frac{\gamma(k)}{\gamma(0)}
$$

Un modelo AR(1) usa esa dependencia de primer rezago:

$$
X_t=c+\phi X_{t-1}+\varepsilon_t
$$

La condición $|\phi|<1$ asegura estacionariedad débil en el caso AR(1). Esta condición está relacionada con la necesidad de que las autocovarianzas no crezcan sin límite.

### ARIMA

ARIMA combina:

- AR($p$): dependencia con rezagos de la propia serie.
- I($d$): diferenciación para estabilizar media.
- MA($q$): dependencia con errores pasados.

Forma compacta:

$$
\phi(B)(1-B)^dX_t=\theta(B)\varepsilon_t
$$

donde $B$ es el operador de rezago.

### ARIMAX

ARIMAX agrega covariables externas:

$$
X_t=c+\beta^TZ_t+\text{estructura ARIMA}+\varepsilon_t
$$

En demanda, $Z_t$ puede incluir precio, promoción o precio de competidor. La interpretación de $\beta$ debe separar efecto contemporáneo de dependencia temporal.

### VAR

VAR modela varias series simultáneamente:

$$
\mathbf{X}_t=A_1\mathbf{X}_{t-1}+\cdots+A_p\mathbf{X}_{t-p}+\varepsilon_t
$$

Las matrices $A_i$ cuantifican cómo un shock en una serie se transmite a otras. Es una extensión natural de momentos conjuntos y matrices de covarianza hacia dependencia temporal multivariada.
