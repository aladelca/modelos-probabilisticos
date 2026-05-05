# Sesión 08 - Modelos continuos y teorema del límite central

## Logro de la sesión

Al finalizar la sesión, el estudiante elabora modelos para variables continuas, interpreta sus supuestos y usa el teorema del límite central para aproximar distribuciones de promedios.

## Modelos continuos frecuentes

### Uniforme

Todos los valores en un intervalo $[a,b]$ tienen la misma densidad:

$$
X\sim Uniforme(a,b)
$$

Útil como modelo simple de incertidumbre acotada.

### Normal

$$
X\sim N(\mu,\sigma^2)
$$

Es simétrica y aparece como aproximación de muchos promedios. No es adecuada para variables estrictamente positivas con cola derecha fuerte si se usa sin transformación.

### Exponencial

Modela tiempos entre eventos bajo tasa constante:

$$
f(x)=\lambda e^{-\lambda x},\quad x\ge0
$$

Tiene propiedad de falta de memoria.

### Gamma

Generaliza tiempos de espera acumulados. Es flexible para variables positivas asimétricas.

### Lognormal

Si $\log X$ es normal, entonces $X$ es lognormal. Útil para montos, ingresos, duración y precios positivos con cola derecha.

### Beta

Modela proporciones entre 0 y 1.

## Teorema del límite central

Si $X_1,\ldots,X_n$ son independientes e idénticamente distribuidas con media $\mu$ y varianza finita $\sigma^2$, entonces:

$$
\frac{\bar{X}-\mu}{\sigma/\sqrt{n}}\approx N(0,1)
$$

para $n$ suficientemente grande.

Implicación: aunque la población original no sea normal, el promedio muestral puede aproximarse por una normal.

## Flujo de modelado

1. Entender soporte y unidad de medida.
2. Visualizar histograma, densidad y cuantiles.
3. Proponer una distribución candidata.
4. Estimar parámetros.
5. Comparar CDF empírica vs teórica.
6. Calcular probabilidades relevantes para la decisión.
7. Documentar supuestos y límites.

## Trabajo final sugerido

Elaborar un modelo probabilístico para un caso aplicado. El informe debe incluir pregunta de negocio, variable aleatoria, supuestos, distribución elegida, estimación de parámetros, validación, simulación e interpretación de probabilidades.

## Profundización para nivel de maestría

### Soporte antes que forma

La primera decisión al modelar una variable continua es el soporte:

- $X\in\mathbb{R}$: normal puede ser candidata.
- $X\ge0$: exponencial, gamma, Weibull, lognormal.
- $0<X<1$: beta.
- $X\in[a,b]$: uniforme, triangular, beta reescalada.
- Variable positiva con masa en cero: modelo mixto o hurdle.

Usar una normal para tiempos o montos puede producir probabilidades negativas imposibles, aunque el ajuste visual parezca aceptable cerca del centro.

### Normal: importancia y límites

La normal es central por tres razones:

1. Aparece como límite de promedios bajo el TLC.
2. Es matemáticamente conveniente.
3. Muchos errores agregados tienden a ser aproximadamente simétricos.

Pero sus colas pueden ser demasiado livianas para riesgo extremo, montos financieros, tiempos operativos o demanda irregular. En esos casos, los cuantiles altos se subestiman.

### Exponencial, Gamma y Weibull

La exponencial tiene tasa de riesgo constante:

$$
h(x)=\lambda
$$

La Gamma permite mayor flexibilidad en forma y representa suma de tiempos de espera. La Weibull permite riesgo creciente o decreciente:

- Forma $k>1$: riesgo aumenta con el tiempo.
- Forma $k<1$: riesgo disminuye.
- Forma $k=1$: caso exponencial.

Estas distribuciones son útiles en confiabilidad, churn, duración de procesos y salud.

### Lognormal y transformaciones

Si:

$$
\log X\sim N(\mu,\sigma^2)
$$

entonces $X$ es lognormal. Sus momentos son:

$$
E[X]=\exp(\mu+\sigma^2/2)
$$

$$
Var(X)=(e^{\sigma^2}-1)e^{2\mu+\sigma^2}
$$

La media supera a la mediana:

$$
Mediana(X)=e^\mu
$$

Esto ayuda a explicar por qué en variables positivas asimétricas el "promedio" puede no representar al caso típico.

### Beta para proporciones

La Beta tiene soporte $(0,1)$ y gran flexibilidad de forma:

$$
X\sim Beta(\alpha,\beta)
$$

$$
E[X]=\frac{\alpha}{\alpha+\beta}
$$

Se usa para tasas, proporciones, scores calibrados, incertidumbre de conversión y prior bayesiano de Bernoulli.

### Estimación y comparación

Métodos comunes:

- Método de momentos: iguala momentos teóricos con empíricos.
- Máxima verosimilitud: maximiza probabilidad de observar los datos.
- Inferencia bayesiana: combina prior y likelihood.
- Bootstrap: aproxima incertidumbre de estimadores por remuestreo.

Para comparar modelos:

- AIC/BIC si se trabaja con likelihood.
- Validación predictiva si importa predicción.
- QQ plot y CDF empírica si importa forma completa.
- Métricas de cola si importan percentiles extremos.

### Teorema del límite central con condiciones

La versión clásica requiere variables i.i.d. con media y varianza finitas. Si hay dependencia fuerte, varianza infinita o muestras con colas extremas, la aproximación puede fallar o requerir tamaños muestrales mucho mayores.

Forma estandarizada:

$$
Z_n=\frac{\bar{X}_n-\mu}{\sigma/\sqrt{n}}\Rightarrow N(0,1)
$$

La convergencia es en distribución, no significa que la población sea normal ni que cualquier muestra pequeña produzca un promedio normal.

### Error estándar

La desviación estándar del promedio es:

$$
SE(\bar{X})=\frac{\sigma}{\sqrt{n}}
$$

En práctica $\sigma$ se reemplaza por $s$:

$$
\widehat{SE}(\bar{X})=\frac{s}{\sqrt{n}}
$$

Esta cantidad mide incertidumbre del estimador, no dispersión de observaciones individuales.

### Intervalos aproximados

Con muestra grande:

$$
\bar{X}\pm z_{1-\alpha/2}\frac{s}{\sqrt{n}}
$$

aproxima un intervalo de confianza para $\mu$. Para muestras pequeñas normales se usa t de Student. Si la distribución es muy asimétrica, puede ser preferible bootstrap o transformar la variable.

### Delta method

Si $\hat{\theta}$ es aproximadamente normal y se necesita una función $g(\hat{\theta})$:

$$
g(\hat{\theta})\approx N\left(g(\theta), [g'(\theta)]^2 Var(\hat{\theta})\right)
$$

Esto aparece al construir intervalos para odds ratios, tasas transformadas, elasticidades, log-medias y métricas derivadas.

### Bootstrap como alternativa práctica

El bootstrap estima la distribución de un estadístico remuestreando con reemplazo. Sirve cuando:

- El estimador es complejo.
- La aproximación normal es dudosa.
- Se requiere intervalo para mediana, percentil, ratio o métrica no lineal.

No arregla datos sesgados ni muestras no representativas; replica la incertidumbre bajo el mecanismo empírico observado.

### Plantilla de trabajo final

Un informe final robusto debería tener:

- Pregunta de decisión y unidad de análisis.
- Definición de variable aleatoria y soporte.
- Análisis exploratorio de distribución.
- Modelos candidatos y justificación de supuestos.
- Estimación de parámetros.
- Validación visual, numérica y de negocio.
- Probabilidades o cuantiles relevantes.
- Simulación de escenarios.
- Sensibilidad ante supuestos.
- Limitaciones y recomendación accionable.

## Funciones de pérdida para demanda y variables positivas

La función de pérdida define qué aspecto de la distribución condicional aprende el modelo.

### Pérdida cuadrática

$$
L(y,\hat{y})=(y-\hat{y})^2
$$

Minimizar pérdida cuadrática estima la media condicional $E[Y\mid X]$. Es sensible a outliers y penaliza fuertemente errores grandes.

### Pérdida absoluta y cuantiles

La pérdida absoluta:

$$
L(y,\hat{y})=|y-\hat{y}|
$$

estima la mediana condicional. La pinball loss estima cuantiles:

$$
L_\tau(y,\hat{y})=
\begin{cases}
\tau(y-\hat{y}),& y\ge \hat{y}\\
(1-\tau)(\hat{y}-y),& y<\hat{y}
\end{cases}
$$

Esto es útil para inventario, capacidad y escenarios pesimista/base/optimista.

### Poisson y Gamma

Para conteos, una pérdida Poisson es coherente con:

$$
Y\mid X\sim Poisson(\lambda(X))
$$

Para variables positivas continuas y asimétricas, una pérdida Gamma puede ser más adecuada:

$$
Y\mid X\sim Gamma(\text{media dependiente de }X,\ \text{forma})
$$

En el notebook se comparan `squared_error`, `poisson`, `gamma`, cuantiles y transformaciones del target (`log1p`, raíz cuadrada). La comparación se hace con RMSE, MAE, sesgo y error absoluto p90.
