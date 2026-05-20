---
marp: true
theme: default
paginate: true
math: mathjax
backgroundColor: #f8fafc
color: #111827
style: |
  section {
    background: #f8fafc;
    color: #111827;
    font-size: 23px;
    letter-spacing: 0;
  }
  h1, h2, h3 {
    color: #0f172a;
  }
  h1 {
    font-size: 2.25em;
  }
  h2 {
    font-size: 1.50em;
  }
  h3 {
    font-size: 1.10em;
  }
  strong {
    color: #075985;
  }
  code, pre {
    background: #e2e8f0;
    color: #0f172a;
  }
  pre {
    font-size: 0.78em;
  }
  section table {
    width: 100% !important;
    border-collapse: collapse !important;
    font-size: 0.72em !important;
    color: #111827 !important;
    background: transparent !important;
  }
  section table thead,
  section table tbody,
  section table tr {
    background: transparent !important;
  }
  section table th,
  section table td {
    border: 1.5px solid #475569 !important;
    padding: 0.28em 0.40em !important;
    text-shadow: none !important;
    opacity: 1 !important;
  }
  section table th {
    background: #0f172a !important;
    color: #ffffff !important;
    font-weight: 800 !important;
  }
  section table td {
    background: #ffffff !important;
    color: #111827 !important;
  }
  section table tr:nth-child(even) td {
    background: #e2e8f0 !important;
    color: #111827 !important;
  }
  section table code {
    color: #0f172a !important;
    background: #cbd5e1 !important;
  }
  section table th mjx-container,
  section table th mjx-container * {
    color: #ffffff !important;
  }
  section table td mjx-container,
  section table td mjx-container * {
    color: #111827 !important;
  }
  section.small {
    font-size: 19px;
  }
  section.tiny {
    font-size: 16px;
  }
---

# Modelos Probabilísticos

## Semana 3: Variables aleatorias, distribuciones inducidas y Multinomial Naive Bayes

Maestría en Data Science

---

## Logro de la sesión

Al finalizar la sesión, el estudiante:

- Distingue resultados aleatorios, eventos y variables aleatorias.
- Formaliza variables aleatorias como funciones sobre un espacio muestral.
- Diferencia variables discretas, continuas y mixtas.
- Construye PMF, PDF, CDF y cuantiles.
- Implementa MultinomialNB desde sus fórmulas probabilísticas.
- Usa transformaciones para formular variables analizables en código.

---

## Mapa de la clase

1. Seguimiento de sesión 2: de Bayes a MultinomialNB.
2. Implementación fórmula por fórmula de MultinomialNB.
3. Apertura: variables aleatorias en fenómenos cotidianos.
4. Definición formal y distribución inducida.
5. Variables discretas: soporte, PMF y CDF.
6. Variables continuas: densidad, intervalos y cuantiles.
7. Transformaciones, indicadores y estandarización.
8. Ejercicios guiados con soluciones.

---

## Seguimiento de sesión 2

La sesión anterior cerró con Bayes:

$$
P(C_k \mid x)=\frac{P(x \mid C_k)P(C_k)}{P(x)}
$$

En clasificación:

- $C_k$ es una clase.
- $x$ es el vector observado.
- $P(C_k)$ es la probabilidad base de la clase.
- $P(x \mid C_k)$ mide qué tan compatible es el dato con esa clase.

---

## Decisión Bayesiana

Para clasificar, no siempre necesitamos el denominador:

$$
\hat{c}=\arg\max_{c}P(C_c \mid x)
$$

Como $P(x)$ no depende de la clase:

$$
\hat{c}=\arg\max_{c}P(x \mid C_c)P(C_c)
$$

La dificultad real es modelar $P(x \mid C_c)$.

---

## Supuesto Naive

Si $x=(x_1,\ldots,x_d)$, la regla de la cadena dice:

$$
P(x \mid C_c)=P(x_1,\ldots,x_d \mid C_c)
$$

Naive Bayes aproxima:

$$
P(x \mid C_c)\approx \prod_{j=1}^{d}P(x_j \mid C_c)
$$

El supuesto fuerte es independencia condicional de los predictores dada la clase.

---

## Tres variantes comunes

| Variante | Tipo de $x_j$ | Modelo para $P(x_j \mid C_c)$ | Uso típico |
|---|---|---|---|
| GaussianNB | Continuo | Densidad normal | Edad, tarifa, score, tiempo |
| CategoricalNB | Categoría | Tabla de probabilidades | Color, región, canal |
| MultinomialNB | Conteo | Multinomial | Texto, frecuencia de eventos, canastas |

La sesión 3 usa MultinomialNB porque obliga a conectar conteos, probabilidades y variables aleatorias discretas.

---

## MultinomialNB: qué modela

Un registro se representa como vector de conteos:

$$
x_i=(x_{i1},x_{i2},\ldots,x_{id})
$$

donde:

- $x_{ij}\in\mathbb{N}_0$.
- $d$ es el tamaño del vocabulario o número de features de conteo.
- $n_i=\sum_{j=1}^{d}x_{ij}$ es el total de ocurrencias del registro.

Ejemplo: número de veces que aparecen ciertas palabras en un comentario.

---

## Datos de entrenamiento

Tenemos pares:

$$
\mathcal{D}=\{(x_i,y_i)\}_{i=1}^{N}
$$

con:

$$
y_i\in\{1,\ldots,K\}
$$

Para cada clase $c$, definimos:

$$
N_c=\sum_{i=1}^{N}\mathbb{1}(y_i=c)
$$

$N_c$ cuenta cuántos registros pertenecen a la clase $c$.

---

## Paso 1: prior de clase

Estimador de máxima verosimilitud:

$$
\hat{\pi}_c=P(Y=c)=\frac{N_c}{N}
$$

Lectura:

- Si la clase es frecuente, su prior sube.
- Si hay desbalance, el prior puede dominar cuando la evidencia es débil.
- El prior no usa las palabras ni los conteos; solo la etiqueta.

---

## Paso 2: conteos por clase y feature

Para cada clase $c$ y feature $j$:

$$
N_{cj}=\sum_{i:y_i=c}x_{ij}
$$

$N_{cj}$ cuenta la masa total del feature $j$ dentro de la clase $c$.

Luego:

$$
N_{c\cdot}=\sum_{j=1}^{d}N_{cj}
$$

es el total de tokens, eventos o conteos observados en la clase.

---

## Paso 3: probabilidad de feature dado clase

Sin suavizado:

$$
\hat{\theta}_{cj}=\frac{N_{cj}}{N_{c\cdot}}
$$

Problema:

- Si $N_{cj}=0$, entonces $\hat{\theta}_{cj}=0$.
- Una sola palabra no vista puede hacer que toda la verosimilitud sea cero.

Por eso se usa suavizado.

---

## Paso 4: suavizado de Laplace

Con $\alpha>0$:

$$
\hat{\theta}_{cj}=\frac{N_{cj}+\alpha}{N_{c\cdot}+\alpha d}
$$

Interpretación:

- Agregamos $\alpha$ pseudo-conteos a cada feature.
- El denominador agrega $\alpha d$ para mantener suma 1.
- $\alpha=1$ es Laplace; $\alpha<1$ es suavizado más débil.

---

## Paso 5: likelihood multinomial

Si $x=(x_1,\ldots,x_d)$ y $n=\sum_j x_j$:

$$
P(x \mid Y=c)=
\frac{n!}{\prod_{j=1}^{d}x_j!}
\prod_{j=1}^{d}\theta_{cj}^{x_j}
$$

La primera parte es el coeficiente multinomial.

La segunda parte acumula evidencia feature por feature.

---

## Paso 5.1: insertar en Bayes

Para clasificar:

$$
\hat{c}
=
\arg\max_c P(Y=c\mid x)
$$

Por Bayes:

$$
P(Y=c\mid x)
=
\frac{P(x\mid Y=c)P(Y=c)}{P(x)}
$$

Como $P(x)$ es igual para todas las clases:

$$
\hat{c}
=
\arg\max_c P(x\mid Y=c)P(Y=c)
$$

---

## Paso 5.2: reemplazar la likelihood

Usamos:

$$
P(x \mid Y=c)=
\frac{n!}{\prod_{j=1}^{d}x_j!}
\prod_{j=1}^{d}\hat{\theta}_{cj}^{x_j}
$$

Entonces:

$$
\hat{c}
=
\arg\max_c
\left[
\hat{\pi}_c
\frac{n!}{\prod_{j=1}^{d}x_j!}
\prod_{j=1}^{d}\hat{\theta}_{cj}^{x_j}
\right]
$$

---

## Paso 5.3: identificar constantes

Para un documento fijo $x$:

$$
K(x)=\frac{n!}{\prod_{j=1}^{d}x_j!}
$$

no depende de la clase $c$.

Por tanto:

$$
\arg\max_c
\left[
\hat{\pi}_c K(x)
\prod_{j=1}^{d}\hat{\theta}_{cj}^{x_j}
\right]
=
\arg\max_c
\left[
\hat{\pi}_c
\prod_{j=1}^{d}\hat{\theta}_{cj}^{x_j}
\right]
$$

El coeficiente se elimina solo para decidir la clase; no para calcular la probabilidad exacta del vector $x$.

---

## Paso 5.4: aplicar logaritmo

El logaritmo es estrictamente creciente:

$$
\arg\max_c a_c
=
\arg\max_c \log(a_c)
$$

si $a_c>0$.

Entonces:

$$
\arg\max_c
\left[
\hat{\pi}_c
\prod_{j=1}^{d}\hat{\theta}_{cj}^{x_j}
\right]
=
\arg\max_c
\left[
\log \hat{\pi}_c
+
\log
\prod_{j=1}^{d}\hat{\theta}_{cj}^{x_j}
\right]
$$

---

## Paso 5.5: producto a suma

Usamos dos propiedades:

$$
\log\prod_{j=1}^{d}a_j
=
\sum_{j=1}^{d}\log a_j
$$

$$
\log(a^b)=b\log a
$$

Por tanto:

$$
\log
\prod_{j=1}^{d}\hat{\theta}_{cj}^{x_j}
=
\sum_{j=1}^{d}x_j\log \hat{\theta}_{cj}
$$

Los términos con $x_j=0$ no aportan al score.

---

## Paso 6: score para clasificación

Juntando los pasos anteriores:

$$
s_c=
\log \hat{\pi}_c
+
\sum_{j=1}^{d}x_j\log \hat{\theta}_{cj}
$$

Esto es equivalente a maximizar:

$$
\hat{\pi}_c
\frac{n!}{\prod_j x_j!}
\prod_j\hat{\theta}_{cj}^{x_j}
$$

pero es más estable numéricamente y más simple de calcular.

Predicción:

$$
\hat{c}=\arg\max_c s_c
$$

Trabajar en log evita underflow con productos de muchas probabilidades pequeñas.

---

## Paso 7: posterior normalizado

Si queremos probabilidades:

$$
P(Y=c\mid x)=
\frac{\exp(s_c)}
{\sum_{k=1}^{K}\exp(s_k)}
$$

En código se usa log-sum-exp:

$$
\log\sum_k \exp(s_k)
=
m+\log\sum_k \exp(s_k-m)
$$

donde $m=\max_k s_k$.

---

## Ejemplo pequeño

Vocabulario:

$$
[\text{cafe},\text{frio},\text{fila},\text{rapido}]
$$

Clases:

$$
Y\in\{\text{queja},\text{no\_queja}\}
$$

Conteos agregados:

| Clase | cafe | frio | fila | rapido | Total |
|---|---:|---:|---:|---:|---:|
| queja | 4 | 5 | 3 | 0 | 12 |
| no_queja | 3 | 0 | 1 | 5 | 9 |

---

## Ejemplo: prior y suavizado

Supongamos 3 documentos por clase:

$$
\hat{\pi}_{queja}=\hat{\pi}_{no\_queja}=0.5
$$

Con $\alpha=1$ y $d=4$:

$$
\hat{\theta}_{queja}
=
\left(\frac{5}{16},\frac{6}{16},\frac{4}{16},\frac{1}{16}\right)
$$

$$
\hat{\theta}_{no\_queja}
=
\left(\frac{4}{13},\frac{1}{13},\frac{2}{13},\frac{6}{13}\right)
$$

---

## Ejemplo: documento nuevo

Documento:

$$
x=[1,1,1,0]
$$

Representa una ocurrencia de cafe, frio y fila.

Scores:

$$
s_{queja}=
\log(0.5)+
\log(5/16)+\log(6/16)+\log(4/16)
\approx -4.22
$$

$$
s_{no\_queja}=
\log(0.5)+
\log(4/13)+\log(1/13)+\log(2/13)
\approx -6.31
$$

---

## Ejemplo: posterior

Normalizando:

$$
P(queja\mid x)
=
\frac{\exp(-4.22)}
{\exp(-4.22)+\exp(-6.31)}
\approx 0.889
$$

$$
P(no\_queja\mid x)\approx 0.111
$$

El modelo clasifica como queja porque las palabras frio y fila tienen alta probabilidad bajo esa clase.

---

## Implementación fórmula por fórmula

```python
X = np.array([
    [4, 5, 3, 0],  # conteos agregados de queja
    [3, 0, 1, 5],  # conteos agregados de no_queja
])

class_count = np.array([3, 3])
alpha = 1.0
d = X.shape[1]

pi = class_count / class_count.sum()
theta = (X + alpha) / (X.sum(axis=1, keepdims=True) + alpha * d)
```

`pi` implementa $\hat{\pi}_c$ y `theta` implementa $\hat{\theta}_{cj}$.

---

## Implementación del score

```python
x_new = np.array([1, 1, 1, 0])

log_prior = np.log(pi)
log_likelihood = x_new @ np.log(theta).T
scores = log_prior + log_likelihood

posterior = np.exp(scores - scores.max())
posterior = posterior / posterior.sum()
prediction = posterior.argmax()
```

Correspondencia:

$$
s_c=\log \hat{\pi}_c+\sum_j x_j\log \hat{\theta}_{cj}
$$

---

## Qué diagnosticar en MultinomialNB

| Riesgo | Síntoma | Diagnóstico |
|---|---|---|
| Ceros estructurales | Scores extremos | Revisar $\alpha$ y vocabulario |
| Leakage | Métricas demasiado altas | Separar vectorizador fit/transform |
| Desbalance | Clase mayoritaria domina | Priors, matriz de confusión |
| Mala calibración | Probabilidades sobreconfiadas | Curva de calibración, log-loss |
| Dependencia fuerte | Errores sistemáticos | Comparar con modelos discriminativos |

---

## Apertura de variables aleatorias

El PDF original abre con una idea didáctica:

- Observamos fenómenos cotidianos con incertidumbre.
- Asignamos números a resultados posibles.
- Graficamos y modelamos esos números.

Ejemplo operativo:

$$
X=
\begin{cases}
0,& \text{cafe frio}\\
1,& \text{cafe tibio}\\
2,& \text{cafe caliente}
\end{cases}
$$

La variable aleatoria no es el cafe; es la codificación numérica del estado observado.

---

## Variables en la vida diaria

Ejemplos para discusión inicial:

| Fenómeno | Variable aleatoria | Tipo |
|---|---|---|
| Transporte | Tiempo de viaje en minutos | Continua |
| Comunicaciones | Correos recibidos por día | Discreta |
| Servicio | Veces que falla una conexión | Discreta |
| Compra | Monto gastado en una visita | Mixta |
| Operaciones | Demanda diaria de un producto | Conteo |

La pregunta clave no es solo "qué dato tengo", sino qué experimento aleatorio estoy modelando.

---

## De resultado a variable

Un experimento aleatorio tiene resultados elementales:

$$
\omega\in\Omega
$$

Una variable aleatoria asigna un número a cada resultado:

$$
X:\Omega\rightarrow\mathbb{R}
$$

La misma $\omega$ puede producir muchas variables:

- monto comprado,
- indicador de compra,
- categoría comprada,
- tiempo hasta comprar.

---

## Definición formal

Una variable aleatoria es una función medible:

$$
X:(\Omega,\mathcal{F})\rightarrow(\mathbb{R},\mathcal{B})
$$

donde:

- $\Omega$ es el espacio muestral.
- $\mathcal{F}$ contiene los eventos medibles.
- $\mathcal{B}$ contiene los conjuntos borelianos de los reales.

La condición medible asegura que eventos como $\{X\le x\}$ tengan probabilidad definida.

---

## Distribución inducida

La variable $X$ induce una distribución sobre los reales:

$$
P_X(B)=P(\{\omega\in\Omega:X(\omega)\in B\})
$$

para todo conjunto medible $B\subseteq\mathbb{R}$.

Esto explica por qué podemos dejar de trabajar con $\Omega$ y analizar:

- histogramas,
- PMF,
- PDF,
- CDF,
- cuantiles.

---

## Ejemplo: dos dados

Resultado elemental:

$$
\omega=(i,j),\quad i,j\in\{1,\ldots,6\}
$$

Espacio muestral:

$$
|\Omega|=36
$$

Variables posibles:

$$
X(i,j)=i+j
$$

$$
Y(i,j)=\max(i,j)
$$

$$
Z(i,j)=\mathbb{1}(i=j)
$$

---

## Variable discreta

$X$ es discreta si su soporte es finito o contable:

$$
\mathcal{X}=\{x:P(X=x)>0\}
$$

Su función de masa de probabilidad es:

$$
p_X(x)=P(X=x)
$$

Debe cumplir:

$$
p_X(x)\ge 0,\qquad \sum_{x\in\mathcal{X}}p_X(x)=1
$$

---

## PMF de la suma de dos dados

Para $X=i+j$:

| $x$ | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| casos | 1 | 2 | 3 | 4 | 5 | 6 | 5 | 4 | 3 | 2 | 1 |
| $p_X(x)$ | 1/36 | 2/36 | 3/36 | 4/36 | 5/36 | 6/36 | 5/36 | 4/36 | 3/36 | 2/36 | 1/36 |

La PMF se obtiene contando preimágenes:

$$
P(X=x)=\frac{|\{\omega:X(\omega)=x\}|}{36}
$$

---

## CDF discreta

Para cualquier variable:

$$
F_X(x)=P(X\le x)
$$

En el caso discreto:

$$
F_X(x)=\sum_{t\le x}p_X(t)
$$

La CDF de una variable discreta es escalonada. Cada salto representa masa puntual.

---

## Indicadores

Para cualquier evento $A$:

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

Esta identidad conecta probabilidad, esperanza, métricas binarias y simulación Monte Carlo.

---

## Variable continua

$X$ es continua si sus probabilidades se describen por una densidad:

$$
f_X(x)\ge 0
$$

$$
\int_{-\infty}^{\infty}f_X(x)\,dx=1
$$

Las probabilidades se calculan como áreas:

$$
P(a\le X\le b)=\int_a^b f_X(x)\,dx
$$

---

## Probabilidad puntual vs densidad

Para una variable continua:

$$
P(X=x)=0
$$

pero:

$$
f_X(x)
$$

puede ser mayor que 1 en algunos puntos.

La densidad no es probabilidad. La probabilidad aparece al integrar sobre un intervalo.

---

## Ejemplo continuo: tiempo de espera

Si el tiempo hasta un evento sigue distribución exponencial:

$$
T\sim Exponencial(\lambda)
$$

entonces:

$$
f_T(t)=\lambda e^{-\lambda t},\quad t\ge 0
$$

$$
F_T(t)=1-e^{-\lambda t}
$$

La esperanza es:

$$
E[T]=\frac{1}{\lambda}
$$

---

## Cálculo de intervalo

Si el tiempo promedio de espera es 8 minutos:

$$
\lambda=\frac{1}{8}
$$

La probabilidad de esperar entre 5 y 10 minutos:

$$
P(5\le T\le 10)=F_T(10)-F_T(5)
$$

$$
=e^{-5/8}-e^{-10/8}\approx 0.249
$$

---

## Cuantiles

El cuantil de orden $q$ se define como:

$$
F_X^{-1}(q)=\inf\{x:F_X(x)\ge q\}
$$

Interpretación:

- Mediana: $q=0.5$.
- Percentil 95: umbral que deja 95% de masa acumulada.
- En operaciones: nivel de inventario para cubrir una fracción objetivo de demanda.

---

## Transformaciones

Si $Y=g(X)$, entonces $Y$ también es variable aleatoria.

Ejemplos:

$$
Z=\frac{X-\mu}{\sigma}
$$

$$
Y=\log(X)
$$

$$
I=\mathbb{1}(X>c)
$$

La transformación cambia soporte, escala, simetría y a veces el modelo apropiado.

---

## Cambio de variable

Si $Y=g(X)$, con $g$ monótona y diferenciable:

$$
f_Y(y)=f_X(g^{-1}(y))
\left|
\frac{d}{dy}g^{-1}(y)
\right|
$$

Ejemplo:

Si $X$ es positiva y sesgada a la derecha, $Y=\log X$ puede aproximarse mejor a una normal.

---

## Distribuciones mixtas

Muchos datos reales no son puramente discretos ni continuos.

Ejemplos:

- Gasto mensual con masa en cero y monto positivo continuo.
- Tiempo de espera con atención inmediata y cola continua.
- Reclamos con exceso de ceros y conteos positivos.

Un modelo continuo simple puede subestimar la probabilidad de no ocurrencia.

---

## Formulación de variable aleatoria

Para definir una variable en un caso de maestría:

| Elemento | Pregunta |
|---|---|
| Unidad de análisis | ¿cliente, tienda, producto, día, ticket? |
| Horizonte | ¿por minuto, día, campaña, ciclo? |
| Soporte | ¿valores posibles y restricciones? |
| Tipo | ¿discreta, continua, mixta, ordinal? |
| Decisión | ¿qué acción se toma usando la variable? |
| Diagnóstico | ¿qué supuesto puede fallar? |

---

## Conexión con código

En el notebook de semana 3, cada bloque debe explicitar:

| Antes de programar | Pregunta matemática |
|---|---|
| Distribución asumida | ¿PMF, PDF, CDF o simulación empírica? |
| Parámetro estimado | ¿media, $\lambda$, soporte, conteos? |
| Supuesto crítico | ¿equiprobabilidad, independencia, continuidad? |
| Diagnóstico | ¿histograma, CDF, Q-Q plot, estabilidad? |

Esto evita que el notebook sea solo una demo de Python.

---

## Datos de inventario

Dataset:

```text
data_sources/sales_data.csv
```

Unidad de análisis:

$$
(\text{fecha},\text{tienda},\text{producto})
$$

Variables naturales:

- demanda diaria,
- unidades vendidas,
- inventario disponible,
- indicador de promoción,
- precio y precio competidor.

---

## Clasificación de columnas

| Tipo | Columnas |
|---|---|
| Categóricas | `Store ID`, `Product ID`, `Category`, `Region`, `Weather Condition`, `Seasonality` |
| Bernoulli | `Promotion`, `Epidemic` |
| Conteos | `Inventory Level`, `Units Sold`, `Units Ordered`, `Demand` |
| Continuas positivas | `Price`, `Competitor Pricing` |
| Numérica acotada | `Discount` |
| Temporal | `Date` |

La clasificación no es puramente técnica: define qué distribuciones son razonables.

---

## Ejercicio 1

Sobre el lanzamiento de dos dados:

$$
\Omega=\{1,\ldots,6\}^2
$$

Definir:

$$
X=i+j
$$

$$
Y=\max(i,j)
$$

$$
Z=\mathbb{1}(i=j)
$$

Para cada variable: tipo, soporte y PMF.

---

## Solución 1: soportes

| Variable | Tipo | Soporte |
|---|---|---|
| $X=i+j$ | Discreta | $\{2,\ldots,12\}$ |
| $Y=\max(i,j)$ | Discreta | $\{1,\ldots,6\}$ |
| $Z=\mathbb{1}(i=j)$ | Bernoulli | $\{0,1\}$ |

Todas se construyen sobre el mismo experimento, pero responden preguntas distintas.

---

## Solución 1: PMF

Para $Y=\max(i,j)$:

$$
P(Y=m)=\frac{m^2-(m-1)^2}{36}
=\frac{2m-1}{36}
$$

Para $Z=\mathbb{1}(i=j)$:

$$
P(Z=1)=\frac{6}{36}=\frac{1}{6}
$$

$$
P(Z=0)=\frac{5}{6}
$$

---

## Ejercicio 2

Usar:

```text
data_sources/sales_data.csv
```

Preguntas:

1. Clasificar columnas por tipo probabilístico.
2. Proponer tres variables aleatorias para inventario.
3. Explicar qué se pierde al convertir `Demand` en indicador de demanda alta.

---

## Solución 2

Variables aleatorias útiles:

- $D_{s,p,t}$: demanda del producto $p$ en tienda $s$ el día $t$.
- $S_{s,p,t}$: indicador de quiebre de stock.
- $Q_{s,p,t}$: unidades a ordenar para reabastecimiento.

Convertir `Demand` a indicador pierde:

- magnitud,
- cola derecha,
- varianza,
- ranking entre casos altos,
- costo esperado de subabastecimiento.

---

## Ejercicio 3

Simular:

$$
X\sim LogNormal(\mu,\sigma^2)
$$

Comparar:

$$
X
$$

con:

$$
\log X
$$

Pregunta: ¿por qué la transformación puede facilitar un modelo normal?

---

## Solución 3

Por definición:

$$
X\sim LogNormal(\mu,\sigma^2)
\iff
\log X\sim Normal(\mu,\sigma^2)
$$

La transformación log:

- reduce asimetría,
- estabiliza escala,
- acerca la variable transformada a una normal,
- facilita diagnósticos con histograma y Q-Q plot.

---

## Cierre conceptual

La unidad mínima de esta sesión no es el dataframe.

Es la variable aleatoria:

$$
X:\Omega\rightarrow\mathbb{R}
$$

El dataframe aparece después, como observaciones de una o más variables aleatorias bajo supuestos de muestreo, medición y transformación.

---

## Checklist para clase

- Repasar Bayes y Naive Bayes desde probabilidades condicionales.
- Implementar MultinomialNB con conteos, suavizado y log-scores.
- Definir variables aleatorias sobre un mismo experimento.
- Construir PMF y CDF para variables discretas.
- Interpretar PDF como densidad, no como probabilidad puntual.
- Conectar transformaciones matemáticas con código reproducible.

---

## Material asociado

- Teoría: `docs/sesion_03_variables_aleatorias.md`
- Notebook: `notebooks/03_variables_aleatorias.ipynb`
- Ejercicios: `ejercicios/sesion_03.md`
- Soluciones: `soluciones/sesion_03.md`
- Dataset: `data_sources/sales_data.csv`
