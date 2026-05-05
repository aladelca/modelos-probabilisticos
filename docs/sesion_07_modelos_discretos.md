# Sesión 07 - Modelos de probabilidad discretos

## Logro de la sesión

Al finalizar la sesión, el estudiante elabora modelos probabilísticos para variables discretas y evalúa sus propiedades, supuestos y límites.

## Modelos principales

### Bernoulli

Modela un resultado binario:

$$
X\sim Bernoulli(p)
$$

$$
P(X=1)=p,\quad E[X]=p,\quad Var(X)=p(1-p)
$$

Casos: conversión, fraude/no fraude, churn/no churn.

### Binomial

Cuenta éxitos en $n$ ensayos Bernoulli independientes con la misma probabilidad:

$$
X\sim Binomial(n,p)
$$

$$
P(X=k)=\binom{n}{k}p^k(1-p)^{n-k}
$$

$$
E[X]=np,\quad Var(X)=np(1-p)
$$

### Geométrica

Cuenta intentos hasta el primer éxito:

$$
P(X=k)=(1-p)^{k-1}p
$$

Casos: intentos hasta primera compra, llamadas hasta contacto efectivo.

### Poisson

Modela conteos en un intervalo cuando los eventos ocurren con tasa promedio $\lambda$:

$$
P(X=k)=e^{-\lambda}\frac{\lambda^k}{k!}
$$

$$
E[X]=Var(X)=\lambda
$$

Si la varianza observada es mucho mayor que la media, puede haber sobredispersión.

### Binomial negativa

Útil para conteos sobredispersos. Permite:

$$
Var(X)>E[X]
$$

Casos: reclamos por cliente, visitas por usuario, defectos por lote heterogéneo.

### Multinomial

Generaliza la binomial a más de dos categorías:

$$
(X_1,\ldots,X_k)\sim Multinomial(n, p_1,\ldots,p_k)
$$

Casos: categorías de compra, clases de texto, elección de canal.

## Cómo elegir

- Binario: Bernoulli.
- Número de éxitos en ensayos fijos: Binomial.
- Espera hasta primer éxito: Geométrica.
- Conteos con media cercana a varianza: Poisson.
- Conteos con sobredispersión: Binomial negativa.
- Conteos por categorías: Multinomial.

## Propiedades a validar

- Soporte: valores posibles.
- Independencia de ensayos.
- Parámetros constantes o heterogéneos.
- Relación media-varianza.
- Calidad de ajuste a datos observados.

## Actividad formativa

Toma un conteo real o simulado. Ajusta un modelo Poisson y evalúa si la varianza empírica contradice el supuesto $E[X]=Var(X)$. Si hay sobredispersión, compara con binomial negativa.

## Profundización para nivel de maestría

### Modelar es declarar supuestos

Elegir una distribución discreta implica declarar:

- Qué valores son posibles.
- Qué mecanismo produce esos valores.
- Qué parámetros controlan frecuencia, variabilidad y colas.
- Si las observaciones son independientes.
- Si hay heterogeneidad entre unidades.

Un modelo simple puede ser útil aunque sea falso, siempre que sus límites estén claros y su error sea aceptable para la decisión.

### Familia exponencial discreta

Varias distribuciones discretas pertenecen a la familia exponencial:

$$
p(x\mid\theta)=h(x)\exp(\eta(\theta)T(x)-A(\theta))
$$

Esta forma explica por qué tienen estimadores, propiedades y modelos GLM asociados. Bernoulli, Binomial, Poisson y Multinomial son ejemplos centrales.

### Bernoulli, likelihood y entropía cruzada

Para $y_i\in\{0,1\}$:

$$
L(p)=\prod_i p^{y_i}(1-p)^{1-y_i}
$$

La log-verosimilitud negativa es:

$$
-\ell(p)=-\sum_i y_i\log p+(1-y_i)\log(1-p)
$$

Esta es la binary cross-entropy usada en regresión logística y redes neuronales para clasificación binaria. El vínculo entre probabilidad y machine learning es directo: entrenar por cross-entropy equivale a máxima verosimilitud Bernoulli.

### Beta-Bernoulli como actualización bayesiana

Si:

$$
p\sim Beta(\alpha,\beta)
$$

y observamos $s$ éxitos y $f$ fracasos, entonces:

$$
p\mid datos\sim Beta(\alpha+s,\beta+f)
$$

Interpretación: $\alpha$ y $\beta$ actúan como pseudo-conteos. Esto es útil en experimentos A/B, tasas de conversión y problemas con pocos datos.

### Poisson como límite de Binomial

Si $X\sim Binomial(n,p)$, $n$ es grande, $p$ es pequeño y $\lambda=np$ se mantiene constante:

$$
X\approx Poisson(\lambda)
$$

Esto justifica Poisson para eventos raros en intervalos: accidentes, fallas, llamadas, reclamos, clics de baja frecuencia.

### Gamma-Poisson y Binomial Negativa

La Binomial Negativa puede derivarse como mezcla Poisson con tasa heterogénea:

$$
X\mid\lambda\sim Poisson(\lambda), \quad \lambda\sim Gamma(\alpha,\beta)
$$

Al integrar $\lambda$, $X$ sigue una Binomial Negativa. Esta interpretación es importante: la sobredispersión puede venir de heterogeneidad no observada entre clientes, tiendas, productos o periodos.

### Zero-inflated y hurdle models

Muchos conteos reales tienen más ceros que los esperados por Poisson o NegBin. Dos enfoques:

- Zero-inflated: mezcla entre un estado estructural de cero y un proceso de conteo.
- Hurdle: primero modela cero vs positivo, luego modela el conteo positivo truncado.

Ejemplos: compras mensuales, reclamos, visitas, defectos, eventos de fraude.

### Multinomial, softmax y clasificación multiclase

Para una observación con $K$ clases:

$$
P(Y=k\mid x)=\frac{\exp(z_k)}{\sum_{j=1}^K \exp(z_j)}
$$

La pérdida cross-entropy multiclase:

$$
-\sum_k y_k\log \hat{p}_k
$$

es la log-verosimilitud negativa de un modelo categórico/multinomial. Softmax regression y redes neuronales multiclase se apoyan en esta interpretación.

### Dirichlet-Multinomial

Si las probabilidades de categoría son inciertas:

$$
\pi\sim Dirichlet(\alpha_1,\ldots,\alpha_K)
$$

y los conteos son multinomiales, la posterior también es Dirichlet:

$$
\pi\mid datos\sim Dirichlet(\alpha_1+x_1,\ldots,\alpha_K+x_K)
$$

Esta conjugación se usa en clasificación de texto, modelos de tópicos, asignación de canales y smoothing de probabilidades.

### GLM para datos discretos

Los modelos lineales generalizados conectan media y covariables:

$$
g(E[Y\mid X])=X\beta
$$

Ejemplos:

- Bernoulli con link logit: regresión logística.
- Poisson con link log: regresión Poisson.
- Binomial Negativa con link log: conteos sobredispersos.

La elección del link debe respetar soporte y facilitar interpretación. En Poisson:

$$
\log(\lambda_i)=x_i^T\beta
$$

por tanto $\exp(\beta_j)$ es multiplicador de tasa.

### Diagnóstico de modelos discretos

Validaciones mínimas:

- Comparar media y varianza empíricas contra las implicadas por el modelo.
- Revisar exceso de ceros.
- Comparar frecuencias observadas vs esperadas.
- Evaluar colas, no solo centro.
- Usar log-likelihood, AIC/BIC o validación predictiva.
- Revisar calibración si el modelo produce probabilidades.

### Criterios para el trabajo de sesión

Una solución de maestría debe explicar:

- Por qué el soporte del modelo coincide con la variable.
- Qué supuesto de independencia o tasa constante se está usando.
- Cómo se estimaron parámetros.
- Qué evidencia apoya o contradice el modelo.
- Qué modelo alternativo sería razonable.
- Qué decisión cambia al usar el modelo.

## Funciones de costo para clasificación desbalanceada

Para clasificación Bernoulli, la log-loss es la verosimilitud negativa:

$$
L(y,p)=-y\log p-(1-y)\log(1-p)
$$

Esta pérdida es una regla de scoring propia: incentiva probabilidades calibradas. Sin embargo, cuando hay desbalance o costos asimétricos, optimizar log-loss no siempre produce el umbral de decisión correcto.

### Pesos de clase

Una versión ponderada es:

$$
L_w(y,p)=-w_y[y\log p+(1-y)\log(1-p)]
$$

Si $w_1>w_0$, los errores sobre la clase positiva pesan más. Esto puede mejorar recall, pero puede empeorar calibración y aumentar falsos positivos.

### Focal loss

Focal loss reduce el peso de ejemplos fáciles:

$$
FL(p_t)=-\alpha_t(1-p_t)^\gamma\log(p_t)
$$

donde:

$$
p_t=
\begin{cases}
p,& y=1\\
1-p,& y=0
\end{cases}
$$

Si $\gamma=0$, se recupera una log-loss ponderada. Si $\gamma>0$, los ejemplos bien clasificados tienen menor contribución. Es útil cuando hay muchos ejemplos fáciles de la clase mayoritaria.

### Umbral óptimo por costo

Después de estimar probabilidades, la decisión puede usar un umbral distinto de 0.5. Si el costo de falso negativo es $C_{FN}$ y el costo de falso positivo es $C_{FP}$, se evalúa:

$$
C(\tau)=C_{FP}FP(\tau)+C_{FN}FN(\tau)
$$

El notebook busca el $\tau$ que minimiza este costo sobre el conjunto de prueba.
