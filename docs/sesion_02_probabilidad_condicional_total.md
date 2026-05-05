# Sesión 02 - Probabilidad condicional y probabilidad total

## Logro de la sesión

Al finalizar la sesión, el estudiante actualiza probabilidades usando información parcial y reconstruye probabilidades mediante particiones del espacio muestral.

## Probabilidad condicional

La probabilidad condicional mide la probabilidad de $A$ dentro del universo reducido donde $B$ ya ocurrió:

$$
P(A\mid B)=\frac{P(A\cap B)}{P(B)}, \quad P(B)>0
$$

De esta definición se obtiene la regla del producto:

$$
P(A\cap B)=P(A\mid B)P(B)
$$

Para más de dos eventos:

$$
P(A\cap B\cap C)=P(A)P(B\mid A)P(C\mid A\cap B)
$$

## Particiones

Una colección $\{B_1,\ldots,B_k\}$ es una partición de $\Omega$ si:

- $B_i\cap B_j=\emptyset$ para $i\ne j$.
- $B_1\cup \cdots \cup B_k=\Omega$.
- $P(B_i)>0$.

## Teorema de probabilidad total

Si $\{B_i\}$ es una partición:

$$
P(A)=\sum_i P(A\mid B_i)P(B_i)
$$

La idea es descomponer la probabilidad de $A$ por escenarios mutuamente excluyentes.

## Teorema de Bayes como extensión natural

Aunque el sílabo enfatiza probabilidad condicional y probabilidad total, Bayes es una consecuencia directa:

$$
P(B_j\mid A)=\frac{P(A\mid B_j)P(B_j)}{\sum_i P(A\mid B_i)P(B_i)}
$$

Interpretación:

- $P(B_j)$: creencia previa.
- $P(A\mid B_j)$: verosimilitud de observar $A$ bajo el escenario $B_j$.
- $P(B_j\mid A)$: creencia posterior luego de observar $A$.

## Ejemplo guía

Un sistema de diagnóstico tiene:

- Prevalencia de enfermedad: $P(D)=0.02$.
- Sensibilidad: $P(+\mid D)=0.95$.
- Especificidad: $P(-\mid D^c)=0.90$.

Primero se calcula:

$$
P(+)=P(+\mid D)P(D)+P(+\mid D^c)P(D^c)
$$

Luego:

$$
P(D\mid +)=\frac{P(+\mid D)P(D)}{P(+)}
$$

El resultado suele ser menor que la intuición inicial cuando la prevalencia es baja.

## Criterios prácticos

Usa probabilidad condicional cuando:

- Se filtra una población.
- Se observa una señal.
- Se conoce un antecedente.
- Se necesita comparar escenarios.

Usa probabilidad total cuando:

- El evento puede ocurrir por varias rutas.
- Hay segmentos, clases o estados latentes.
- Se quiere pasar de probabilidades condicionales a una probabilidad global.

## Actividad sumativa sugerida

Plantea un caso real con al menos tres escenarios base. Define los priors, las probabilidades condicionales y calcula la probabilidad total de un evento de interés. Luego interpreta cuál escenario domina el resultado y qué información adicional cambiaría más la decisión.

## Profundización para nivel de maestría

### Condicionar es cambiar la medida de referencia

La expresión $P(A\mid B)$ no debe leerse como una fórmula aislada. Condicionar redefine el universo operativo: pasamos de medir $A$ dentro de $\Omega$ a medirlo dentro de $B$. Por eso:

$$
P(A\mid B)=\frac{P(A\cap B)}{P(B)}
$$

es una normalización. Todas las probabilidades condicionales sobre $B$ vuelven a sumar 1 dentro de ese subespacio.

### Regla de la cadena

La regla del producto se extiende a secuencias:

$$
P(A_1\cap A_2\cap\cdots\cap A_n)=P(A_1)P(A_2\mid A_1)P(A_3\mid A_1,A_2)\cdots P(A_n\mid A_1,\ldots,A_{n-1})
$$

Esta factorización es la base de modelos secuenciales, modelos de lenguaje, modelos de supervivencia discretizados y árboles de decisión probabilísticos.

### Probabilidad total como marginalización

El teorema de probabilidad total es una operación de marginalización. Si $B$ es una variable de segmento o estado latente:

$$
P(A)=\sum_b P(A\mid B=b)P(B=b)
$$

En términos de modelado, se está integrando la incertidumbre sobre $B$. En modelos continuos la suma se reemplaza por una integral:

$$
P(A)=\int P(A\mid \theta)p(\theta)\,d\theta
$$

Esta idea conecta con inferencia bayesiana, mezclas gaussianas, modelos jerárquicos y ensambles probabilísticos.

### Bayes en odds y factores de Bayes

Bayes también puede escribirse en términos de odds:

$$
\frac{P(H\mid E)}{P(H^c\mid E)}
=
\frac{P(E\mid H)}{P(E\mid H^c)}
\frac{P(H)}{P(H^c)}
$$

El término:

$$
\frac{P(E\mid H)}{P(E\mid H^c)}
$$

es un factor de actualización: indica cuánto multiplica la evidencia a los odds previos. Esta forma ayuda a interpretar tests médicos, scoring de riesgo, detección de fraude y clasificación probabilística.

### Falacia de la tasa base

Un error frecuente es ignorar $P(H)$. En eventos raros, incluso un test con alta sensibilidad y especificidad puede producir muchos falsos positivos. La pregunta correcta no es solo "qué tan bueno es el test", sino:

$$
P(H\mid +)=\frac{P(+\mid H)P(H)}{P(+\mid H)P(H)+P(+\mid H^c)P(H^c)}
$$

La prevalencia controla el denominador. Esto es crucial en fraude, enfermedades raras, anomalías industriales y clasificación con clases desbalanceadas.

### Simpson y segmentación

Las probabilidades condicionales pueden revertir conclusiones agregadas. Un efecto observado en la población completa puede desaparecer o invertirse al condicionar por segmento:

$$
P(A\mid Tratamiento) > P(A\mid Control)
$$

pero para cada segmento $S=s$:

$$
P(A\mid Tratamiento,S=s) < P(A\mid Control,S=s)
$$

Este fenómeno aparece cuando los pesos de los segmentos difieren entre grupos. En análisis aplicado, toda comparación de tasas debe revisar variables de confusión.

### Valor de la información

La probabilidad condicional es útil cuando cambia decisiones. Si una señal $S$ actualiza los escenarios $\theta$, el valor esperado con información es:

$$
EVI = \sum_s P(S=s)\max_a E[U(a,\theta)\mid S=s]
$$

El valor de la información perfecta compara contra observar $\theta$ sin error:

$$
EVPI = E[\max_a U(a,\theta)]-\max_a E[U(a,\theta)]
$$

Si el valor de información es bajo, recolectar más datos puede no justificar costo operativo.

### Calibración de probabilidades

En modelos predictivos, una probabilidad condicional estimada debe calibrarse:

$$
\hat{p}(x)\approx P(Y=1\mid X=x)
$$

Un modelo está calibrado si, entre casos con score cercano a 0.8, aproximadamente 80% son positivos. Evaluar solo accuracy o AUC no basta cuando las probabilidades alimentan decisiones de riesgo.

### Checklist para casos

Antes de entregar un análisis de probabilidad condicional:

- Define claramente evento observado, hipótesis y partición.
- Verifica que la partición sea exhaustiva y excluyente.
- Presenta priors y likelihoods en una tabla.
- Calcula el denominador con probabilidad total.
- Interpreta el posterior en lenguaje del caso.
- Discute sensibilidad ante cambios en priors o tasas de error.

## Naive Bayes desde cero con Titanic

Naive Bayes convierte la regla de Bayes en un clasificador. Para una clase $Y$ y predictores $X_1,\ldots,X_p$:

$$
P(Y=y\mid X_1=x_1,\ldots,X_p=x_p)
\propto P(Y=y)P(X_1=x_1,\ldots,X_p=x_p\mid Y=y)
$$

El supuesto "naive" es independencia condicional de los predictores dado $Y$:

$$
P(X_1=x_1,\ldots,X_p=x_p\mid Y=y)
=\prod_{j=1}^{p}P(X_j=x_j\mid Y=y)
$$

Entonces:

$$
\hat{y}=\arg\max_y \left[\log P(Y=y)+\sum_j \log P(X_j=x_j\mid Y=y)\right]
$$

En el notebook se implementan dos versiones con Titanic:

- **Categórica**: discretiza edad, tarifa y tamaño familiar; estima tablas $P(X_j=x\mid Y=y)$ con suavizado de Laplace.
- **Gaussiana**: modela variables numéricas con una normal por clase y feature:

$$
P(x_j\mid y=c)=
\frac{1}{\sqrt{2\pi\sigma_{jc}^2}}
\exp\left(-\frac{(x_j-\mu_{jc})^2}{2\sigma_{jc}^2}\right)
$$

El objetivo didáctico no es maximizar accuracy, sino ver que un algoritmo de clasificación puede derivarse directamente de probabilidad condicional, probabilidad total y Bayes.
