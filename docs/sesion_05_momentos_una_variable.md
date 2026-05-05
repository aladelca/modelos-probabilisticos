# Sesión 05 - Momentos de una variable aleatoria

## Logro de la sesión

Al finalizar la sesión, el estudiante resume una distribución mediante momentos, estadísticos de posición, dispersión y expectativa condicional.

## Esperanza

La esperanza es el centro probabilístico de una variable aleatoria.

Discreta:

$$
E[X]=\sum_x x p_X(x)
$$

Continua:

$$
E[X]=\int_{-\infty}^{\infty}x f_X(x)\,dx
$$

No siempre coincide con un valor típico observado, especialmente en distribuciones asimétricas.

## Momentos

Momento crudo de orden $r$:

$$
E[X^r]
$$

Momento central de orden $r$:

$$
E[(X-\mu)^r]
$$

Casos importantes:

- Orden 1: media.
- Orden 2 central: varianza.
- Orden 3 estandarizado: asimetría.
- Orden 4 estandarizado: curtosis.

## Dispersión

Varianza:

$$
Var(X)=E[(X-\mu)^2]=E[X^2]-E[X]^2
$$

Desviación estándar:

$$
\sigma=\sqrt{Var(X)}
$$

Coeficiente de variación:

$$
CV=\frac{\sigma}{\mu}
$$

útil cuando se comparan variables positivas con escalas distintas.

## Estadísticos de posición

- Mediana: divide la distribución en 50% y 50%.
- Cuartiles: dividen en cuatro partes.
- Percentiles: ubican valores respecto a la distribución.
- IQR: $Q_3-Q_1$, medida robusta de dispersión.

La media es sensible a extremos; la mediana y el IQR son más robustos.

## Expectativa condicional

$$
E[Y\mid X=x]
$$

resume el valor promedio de $Y$ dentro del grupo o escenario definido por $X=x$. En modelos predictivos, muchas tareas de regresión estiman una expectativa condicional.

## Actividad formativa

Simula una variable positiva con cola derecha, calcula media, mediana, varianza, asimetría y percentiles. Luego segmenta por una variable categórica y estima una expectativa condicional.

## Profundización para nivel de maestría

### Existencia de momentos

No toda distribución tiene media o varianza finita. Algunas distribuciones de cola pesada tienen:

- Media inexistente.
- Media finita pero varianza infinita.
- Momentos altos inexistentes.

Esto afecta inferencia, intervalos de confianza, optimización y métricas. Antes de resumir una variable con media y desviación estándar, se debe revisar soporte, colas y plausibilidad del segundo momento.

### Momentos crudos, centrales y estandarizados

Momento crudo:

$$
m_r'=E[X^r]
$$

Momento central:

$$
\mu_r=E[(X-E[X])^r]
$$

Momento estandarizado:

$$
\alpha_r=E\left[\left(\frac{X-\mu}{\sigma}\right)^r\right]
$$

La asimetría es $\alpha_3$. La curtosis es $\alpha_4$, y muchas librerías reportan exceso de curtosis $\alpha_4-3$.

### Función generadora de momentos

Cuando existe alrededor de $t=0$, la función generadora de momentos es:

$$
M_X(t)=E[e^{tX}]
$$

Los momentos se obtienen derivando:

$$
E[X^r]=M_X^{(r)}(0)
$$

En inferencia y teoría asintótica, también se usa la función generadora de cumulantes:

$$
K_X(t)=\log M_X(t)
$$

Sus derivadas producen cumulantes: media, varianza, asimetría relacionada y curtosis relacionada.

### Linealidad de la esperanza

La esperanza es lineal incluso sin independencia:

$$
E[aX+bY+c]=aE[X]+bE[Y]+c
$$

Esto explica por qué el valor esperado de un portafolio, una demanda agregada o una pérdida total puede calcularse sumando expectativas aunque las variables estén correlacionadas. La independencia sí importa para varianzas.

### Varianza bajo transformaciones lineales

Para constantes $a,b$:

$$
Var(aX+b)=a^2Var(X)
$$

Para sumas:

$$
Var(X+Y)=Var(X)+Var(Y)+2Cov(X,Y)
$$

Si $X$ e $Y$ son independientes:

$$
Var(X+Y)=Var(X)+Var(Y)
$$

Esta distinción es clave en pronósticos agregados, inventario, riesgo financiero y ensambles.

### Desigualdad de Jensen

Si $\phi$ es convexa:

$$
\phi(E[X])\le E[\phi(X)]
$$

Ejemplos:

- $E[X^2]\ge E[X]^2$, lo que implica varianza no negativa.
- En pérdida cuadrática, la incertidumbre aumenta el costo esperado.
- En utilidad cóncava, $E[u(W)]\le u(E[W])$, lo que formaliza aversión al riesgo.

### Expectativa condicional como proyección

En un nivel más avanzado, $E[Y\mid X]$ puede entenderse como la mejor predicción de $Y$ usando funciones de $X$ bajo pérdida cuadrática:

$$
E[Y\mid X]=\arg\min_{g(X)} E[(Y-g(X))^2]
$$

Esto conecta directamente con regresión. Un modelo de regresión intenta aproximar una esperanza condicional, no necesariamente recuperar una relación causal.

### Ley de esperanza total

Si $X$ segmenta la población:

$$
E[Y]=E[E[Y\mid X]]
$$

En versión discreta:

$$
E[Y]=\sum_x E[Y\mid X=x]P(X=x)
$$

Esta propiedad permite reconstruir promedios globales desde segmentos y detectar cambios por composición.

### Ley de varianza total

$$
Var(Y)=E[Var(Y\mid X)] + Var(E[Y\mid X])
$$

Interpretación:

- $E[Var(Y\mid X)]$: variabilidad dentro de segmentos.
- $Var(E[Y\mid X])$: variabilidad entre segmentos.

Es una base conceptual para ANOVA, modelos jerárquicos, segmentación y explicabilidad de varianza.

### Estimadores muestrales

En datos:

$$
\bar{x}=\frac{1}{n}\sum_i x_i
$$

La varianza muestral insesgada usa $n-1$:

$$
s^2=\frac{1}{n-1}\sum_i (x_i-\bar{x})^2
$$

Para describir una muestra puede usarse $n$; para estimar la varianza poblacional bajo muestreo i.i.d. suele usarse $n-1$.

### Robustez

Cuando hay outliers o colas pesadas:

- Preferir mediana e IQR para posición y dispersión robusta.
- Reportar percentiles, no solo media.
- Considerar transformaciones logarítmicas.
- Usar medias recortadas o winsorizadas cuando tenga sentido de negocio.
- Separar valores extremos reales de errores de captura.

### Requisitos para análisis de momentos

Un buen informe debe responder:

- Qué momentos existen bajo el modelo asumido.
- Qué estadísticos son sensibles a extremos.
- Cómo cambian media y varianza por segmento.
- Si la media representa un caso típico o solo un balance matemático.
- Qué implican los percentiles para decisiones operativas.
