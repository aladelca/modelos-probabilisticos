# Sesión 04 - Distribuciones, PMF y PDF

## Logro de la sesión

Al finalizar la sesión, el estudiante evalúa distribuciones de probabilidad para variables discretas y continuas, interpretando masa, densidad y acumulación.

## Distribución de probabilidad

La distribución de una variable aleatoria describe cómo se reparte la probabilidad sobre sus posibles valores.

Para una variable discreta se usa una PMF:

$$
p_X(x)=P(X=x)
$$

Para una variable continua se usa una PDF:

$$
f_X(x)
$$

y se integran áreas para obtener probabilidades.

## PMF

Una PMF válida cumple:

$$
p_X(x)\ge0,\quad \sum_x p_X(x)=1
$$

Lectura: si $p_X(3)=0.18$, entonces el valor exacto $X=3$ tiene probabilidad 18%.

## PDF

Una PDF válida cumple:

$$
f_X(x)\ge0,\quad \int f_X(x)\,dx=1
$$

La densidad no es una probabilidad puntual. Puede incluso ser mayor que 1 si el intervalo donde se concentra es estrecho. La probabilidad está en el área bajo la curva.

## CDF

La CDF acumula probabilidad:

$$
F_X(x)=P(X\le x)
$$

Para una variable discreta, la CDF tiene saltos. Para una variable continua, suele ser suave.

## Relación entre PDF y CDF

Si $X$ es continua:

$$
F_X(x)=\int_{-\infty}^{x} f_X(t)\,dt
$$

y, cuando existe derivada:

$$
f_X(x)=F_X'(x)
$$

## Comparación práctica

Preguntas que guían la elección:

- ¿La variable cuenta unidades? Usualmente discreta.
- ¿La variable mide tiempo, monto, distancia o proporción continua? Usualmente continua.
- ¿Importa el valor exacto o un rango?
- ¿Hay límites naturales, como 0 o 1?

## Actividad formativa

Construye una PMF para una demanda diaria con valores de 0 a 8 unidades. Luego propón una PDF para tiempos de espera. En ambos casos verifica normalización y calcula una probabilidad acumulada.

## Profundización para nivel de maestría

### Distribución como modelo de incertidumbre

Una distribución no es solo una curva: es una afirmación sobre el mecanismo generador de datos. Al elegir una distribución se está afirmando algo sobre:

- Soporte: valores imposibles y posibles.
- Forma: simetría, asimetría, colas, multimodalidad.
- Dependencia de parámetros: media, varianza, tasa, forma.
- Estabilidad: si los parámetros se mantienen en el tiempo o por segmento.
- Nivel de agregación: evento individual, conteo agregado, promedio muestral.

La pregunta no es "qué distribución se parece al histograma", sino "qué distribución representa razonablemente el proceso y la decisión".

### Masa vs densidad

Para una PMF, $p_X(x)$ es una probabilidad. Para una PDF, $f_X(x)$ es densidad, no probabilidad. La probabilidad se obtiene integrando:

$$
P(a<X\le b)=F(b)-F(a)=\int_a^b f_X(x)\,dx
$$

Un error conceptual frecuente es interpretar $f(10)=0.12$ como "12% de probabilidad de que $X=10$". En una variable continua, la probabilidad puntual es cero bajo modelos absolutamente continuos.

### Función de supervivencia y riesgo

Además de la CDF, muchas aplicaciones usan la función de supervivencia:

$$
S(x)=P(X>x)=1-F(x)
$$

En tiempos de falla o espera, se usa la función de riesgo:

$$
h(x)=\frac{f(x)}{S(x)}
$$

Interpretación: tasa instantánea de ocurrencia dado que el evento no ha ocurrido hasta $x$. Es central en mantenimiento, churn, supervivencia de clientes, crédito y salud.

### Distribución empírica

Dada una muestra $x_1,\ldots,x_n$, la CDF empírica es:

$$
\hat{F}_n(x)=\frac{1}{n}\sum_{i=1}^n I(x_i\le x)
$$

La CDF empírica es una estimación no paramétrica de la distribución. Es más estable que un histograma porque no depende de bins. En validación de modelos, comparar $\hat{F}_n$ contra $F_\theta$ suele ser más informativo que mirar solo medias.

### Histograma, KDE y decisiones de visualización

Un histograma depende del ancho de bin. Una KDE depende del bandwidth. Ambos son útiles, pero pueden inducir lecturas falsas:

- Bins muy anchos ocultan multimodalidad.
- Bins muy estrechos exageran ruido.
- KDE puede asignar densidad a valores imposibles, como negativos para tiempos.
- En muestras pequeñas, la forma visual es inestable.

Para reportes, combina histograma, CDF empírica, boxplot/violín y cuantiles.

### Estimación paramétrica por máxima verosimilitud

Si se asume una familia $f(x\mid\theta)$, los parámetros pueden estimarse maximizando:

$$
L(\theta)=\prod_{i=1}^n f(x_i\mid\theta)
$$

o, más comúnmente:

$$
\ell(\theta)=\sum_{i=1}^n \log f(x_i\mid\theta)
$$

Para una PMF se reemplaza $f$ por $p$. La verosimilitud mide qué tan plausibles son los datos observados bajo cada valor de parámetro.

### Bondad de ajuste

Herramientas usuales:

- QQ plot: compara cuantiles empíricos y teóricos.
- PP plot: compara probabilidades acumuladas.
- KS: distancia máxima entre CDF empírica y teórica.
- Anderson-Darling: da más peso a colas.
- AIC/BIC: compara modelos penalizando complejidad.

Ninguna prueba reemplaza el juicio de dominio. Con muestras grandes, diferencias pequeñas pueden ser estadísticamente significativas pero irrelevantes para decisión.

### Distribuciones condicionadas y truncadas

En datos reales a veces observamos una variable solo si supera un umbral. Por ejemplo, tiempos registrados solo si hubo espera o montos solo si hubo compra. Entonces la distribución observada no es $f(x)$, sino:

$$
f(x\mid x>c)=\frac{f(x)}{P(X>c)},\quad x>c
$$

Ignorar truncamiento o censura produce estimaciones sesgadas.

### Requisitos para una entrega sólida

Un análisis de distribución debe incluir:

- Variable, unidad, soporte y periodo.
- Justificación de PMF/PDF elegida.
- Estimación de parámetros.
- Validación visual y numérica.
- Probabilidades relevantes para la decisión.
- Discusión de colas, truncamiento, ceros, outliers y estabilidad temporal.
