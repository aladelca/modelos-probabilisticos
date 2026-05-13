---
marp: true
theme: default
paginate: true
math: mathjax
backgroundColor: #101014
color: #f7f7fb
style: |
  section {
    background: #101014;
    color: #f7f7fb;
    font-size: 23px;
    letter-spacing: 0;
  }
  h1, h2, h3 {
    color: #ffd166;
  }
  h1 {
    font-size: 2.3em;
  }
  h2 {
    font-size: 1.55em;
  }
  h3 {
    font-size: 1.12em;
  }
  strong {
    color: #8ecae6;
  }
  code, pre {
    background: #1d1d27;
    color: #a8dadc;
  }
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.74em;
    color: #f7f7fb;
  }
  th, td {
    border: 1px solid #4b5563;
    padding: 0.28em 0.38em;
    color: #f7f7fb;
    background: rgba(255, 255, 255, 0.055);
  }
  th {
    background: #ffd166;
    color: #101014;
    font-weight: 700;
  }
  tr:nth-child(even) td {
    background: rgba(255, 255, 255, 0.09);
  }
  table code {
    color: #a8dadc;
    background: #1d1d27;
  }
  table mjx-container {
    color: #f7f7fb;
  }
  section.small {
    font-size: 19px;
  }
  section.tiny {
    font-size: 16px;
  }
---

# Modelos Probabilísticos

## Semana 2: Probabilidad condicional, probabilidad total, Bayes, valor de información y Naive Bayes

Maestría en Data Science

---

## Logro de la sesión

Al finalizar la sesión, el estudiante:

- Actualiza probabilidades usando información parcial.
- Reconstruye probabilidades marginales mediante particiones.
- Aplica Bayes para inferencia inversa.
- Evalúa si una señal tiene valor económico.
- Implementa Naive Bayes desde probabilidad condicional.

---

## Mapa de la clase

1. Probabilidad condicional como cambio de universo.
2. Regla del producto y regla de la cadena.
3. Particiones y teorema de probabilidad total.
4. Teorema de Bayes y tasa base.
5. Actualización secuencial con odds y likelihood ratios.
6. EVPI y EVSI.
7. Naive Bayes categórico y gaussiano.
8. Ejercicios y soluciones.

---

## Idea central

Sin información:

$$
P(A)
$$

Con información observada $B$:

$$
P(A \mid B)
$$

La diferencia no es solo algebraica. Cambia la población de referencia, el denominador y, muchas veces, la decisión óptima.

---

## Ejemplo de motivación

| Escenario | Evento $A$ | Evidencia $B$ | $P(A)$ | $P(B)$ | $P(A \cap B)$ | $P(A \mid B)$ |
|---|---|---|---:|---:|---:|---:|
| Marketing | Compra | Segmento leal | 0.12 | 0.30 | 0.105 | 0.35 |
| Diagnóstico | Enfermedad | Test positivo | 0.01 | 0.03 | 0.0099 | 0.33 |
| Operaciones | Demanda alta | Pronóstico favorable | 0.40 | 0.50 | 0.34 | 0.68 |

$$
P(A \mid B)=\frac{P(A \cap B)}{P(B)}
$$

---

## Probabilidad condicional

Sea $P(B)>0$:

$$
P(A \mid B)=\frac{P(A \cap B)}{P(B)}
$$

Lectura:

- $B$ es la información ya observada.
- $A \cap B$ son los casos donde ocurre lo que buscamos dentro del filtro.
- $P(B)$ renormaliza el universo.

---

## Condicionar como nueva medida

Definimos:

$$
P_B(A)=P(A \mid B)=\frac{P(A \cap B)}{P(B)}
$$

Entonces $P_B$ vuelve a cumplir los axiomas sobre el universo condicionado:

- $P_B(A)\ge 0$.
- $P_B(B)=1$.
- Si los $A_i$ son disjuntos, $P_B(\cup_i A_i)=\sum_i P_B(A_i)$.

Condicionar no siempre reduce una probabilidad. Cambia la referencia.

---

## Regla del producto

De la definición:

$$
P(A \cap B)=P(A \mid B)P(B)
$$

También:

$$
P(A \cap B)=P(B \mid A)P(A)
$$

Por tanto:

$$
P(A \mid B)P(B)=P(B \mid A)P(A)
$$

Esta igualdad es el puente hacia Bayes.

---

## Regla de la cadena

Para tres eventos:

$$
P(A \cap B \cap C)=P(A)P(B \mid A)P(C \mid A,B)
$$

Para $n$ eventos:

$$
P(A_1,\ldots,A_n)=P(A_1)\prod_{j=2}^{n}P(A_j \mid A_1,\ldots,A_{j-1})
$$

Uso: modelos secuenciales, series de eventos, árboles de decisión probabilísticos y modelos de lenguaje.

---

## Error común: independencia no es exclusión

Eventos independientes:

$$
P(A \cap B)=P(A)P(B)
$$

Eventos mutuamente excluyentes:

$$
P(A \cap B)=0
$$

Si $P(A)>0$ y $P(B)>0$, dos eventos excluyentes no pueden ser independientes.

---

## Particiones

Una colección $\{B_1,\ldots,B_k\}$ es partición de $\Omega$ si:

- $B_i \cap B_j=\emptyset$ para $i\ne j$.
- $B_1\cup\cdots\cup B_k=\Omega$.
- $P(B_i)>0$.

Ejemplos:

- Segmento de cliente.
- Estado de demanda.
- Clase real de un caso.
- Resultado de un primer dado.

---

## Teorema de probabilidad total

Si $\{B_i\}$ es una partición:

$$
P(A)=\sum_i P(A \mid B_i)P(B_i)
$$

Interpretación:

- $P(A \mid B_i)$: probabilidad dentro del escenario $i$.
- $P(B_i)$: peso del escenario.
- $P(A)$: promedio ponderado por escenarios.

---

## Forma vectorial

Si:

$$
\ell_i=P(A \mid B_i), \qquad \pi_i=P(B_i)
$$

entonces:

$$
P(A)=\ell^\top \pi
$$

En código:

```python
p_a = np.dot(likelihoods, priors)
```

Esta forma aparece en Bayes, mezclas, ensambles y modelos jerárquicos.

---

## Teorema de Bayes

Para una partición $\{B_i\}$:

$$
P(B_j \mid A)=
\frac{P(A \mid B_j)P(B_j)}
{\sum_i P(A \mid B_i)P(B_i)}
$$

Componentes:

- $P(B_j)$: prior.
- $P(A \mid B_j)$: likelihood.
- $P(B_j \mid A)$: posterior.
- Denominador: evidencia o normalizador.

---

## Derivación de Bayes

Por definición:

$$
P(B_j \mid A)=\frac{P(A \cap B_j)}{P(A)}
$$

Por regla del producto:

$$
P(A \cap B_j)=P(A \mid B_j)P(B_j)
$$

Por probabilidad total:

$$
P(A)=\sum_i P(A \mid B_i)P(B_i)
$$

---

## Flujo bayesiano

1. Definir hipótesis o estados $B_i$.
2. Definir priors $P(B_i)$.
3. Definir likelihoods $P(A \mid B_i)$.
4. Observar evidencia $A$.
5. Calcular posteriors $P(B_i \mid A)$.
6. Tomar decisiones con utilidad esperada.

---

## Caso guía: test médico

Datos:

- Prevalencia: $P(D)=0.02$.
- Sensibilidad: $P(+ \mid D)=0.95$.
- Especificidad: $P(- \mid D^c)=0.90$.
- Falso positivo: $P(+ \mid D^c)=0.10$.

Primero:

$$
P(+)=0.95(0.02)+0.10(0.98)=0.117
$$

---

## Caso guía: posterior

$$
P(D \mid +)=\frac{P(+ \mid D)P(D)}{P(+)}
$$

$$
P(D \mid +)=\frac{0.95(0.02)}{0.117}=0.1624
$$

Lectura:

Un positivo no implica enfermedad con probabilidad cercana a 1. La prevalencia baja pesa fuerte en el denominador.

---

## Falacia de tasa base

Error:

> El test tiene 95% de sensibilidad, entonces un positivo significa 95% de probabilidad de enfermedad.

Corrección:

$$
P(D \mid +)
=
\frac{P(+ \mid D)P(D)}
{P(+ \mid D)P(D)+P(+ \mid D^c)P(D^c)}
$$

La tasa base $P(D)$ no es opcional.

---

## Sensibilidad del posterior

El posterior cambia con:

- Prevalencia.
- Sensibilidad.
- Especificidad.
- Segmentación de la población.

Diagnóstico en notebook:

```python
prevalencias = np.linspace(0.001, 0.30, 100)
posterior = sensibilidad * prev / (
    sensibilidad * prev + (1 - especificidad) * (1 - prev)
)
```

---

## Bayes secuencial

Si llega evidencia $E_1$ y luego $E_2$:

$$
P(H \mid E_1,E_2)=
\frac{P(E_2 \mid H,E_1)P(H \mid E_1)}
{P(E_2 \mid H,E_1)P(H \mid E_1)+P(E_2 \mid H^c,E_1)P(H^c \mid E_1)}
$$

Si las evidencias no son independientes condicionalmente, no se deben multiplicar likelihood ratios como si lo fueran.

---

## Odds y likelihood ratio

Odds previos:

$$
\frac{P(H)}{P(H^c)}
$$

Likelihood ratio:

$$
LR(E)=\frac{P(E \mid H)}{P(E \mid H^c)}
$$

Si $E_1$ y $E_2$ son condicionalmente independientes:

$$
\text{odds posterior}=\text{odds prior}\times LR(E_1)\times LR(E_2)
$$

---

## Valor esperado de una decisión

Con estados $\theta$ y acciones $a$:

$$
E[U(a,\theta)]=\sum_\theta U(a,\theta)P(\theta)
$$

Decisión sin información:

$$
a^*=\arg\max_a E[U(a,\theta)]
$$

Una señal solo sirve si cambia la decisión o mejora el valor esperado neto.

---

## EVPI

Valor esperado con información perfecta:

$$
EV_{\text{perfecta}}=\sum_\theta P(\theta)\max_a U(a,\theta)
$$

Valor esperado sin información:

$$
EV_{\text{sin info}}=\max_a \sum_\theta U(a,\theta)P(\theta)
$$

$$
EVPI=EV_{\text{perfecta}}-EV_{\text{sin info}}
$$

EVPI es el máximo racional a pagar por información perfecta.

---

<!-- _class: small -->

## Ejemplo EVPI: lanzamiento

Estados: Alta $H$ con 0.4 y Baja $L$ con 0.6.

| Acción | Pago si $H$ | Pago si $L$ | EV |
|---|---:|---:|---:|
| Agresiva | 300 | -120 | $0.4(300)+0.6(-120)=48$ |
| Moderada | 180 | 90 | $0.4(180)+0.6(90)=126$ |

Sin información:

$$
EV_{\text{sin info}}=126
$$

Con información perfecta:

$$
EV_{\text{perfecta}}=0.4(300)+0.6(90)=174
$$

$$
EVPI=174-126=48
$$

---

## EVSI

Con señal imperfecta $S$:

$$
P(\theta \mid S=s)=
\frac{P(S=s \mid \theta)P(\theta)}
{\sum_{\theta'}P(S=s \mid \theta')P(\theta')}
$$

Luego:

$$
EV_{\text{con señal}}=
\sum_s P(S=s)\max_a\sum_\theta U(a,\theta)P(\theta \mid S=s)
$$

$$
EVSI=EV_{\text{con señal}}-EV_{\text{sin info}}
$$

---

## Relación entre EVSI y EVPI

Debe cumplirse:

$$
0\le EVSI\le EVPI
$$

Interpretación:

- EVPI: señal perfecta, observa el estado real.
- EVSI: señal ruidosa, observa evidencia imperfecta.
- Si el costo de la señal es mayor que EVSI, no conviene comprarla.

---

## Calibración de probabilidades

Un modelo probabilístico busca:

$$
\hat{p}(x)\approx P(Y=1 \mid X=x)
$$

Calibración:

Entre casos con score cercano a 0.8, aproximadamente 80% deberían ser positivos.

Accuracy y AUC pueden ser buenos aunque las probabilidades estén mal calibradas.

---

## Naive Bayes

Queremos clasificar:

$$
x=(x_1,\ldots,x_p)
$$

Regla de Bayes:

$$
P(Y=y \mid x)\propto P(Y=y)P(x \mid Y=y)
$$

Supuesto naive:

$$
P(x \mid Y=y)=\prod_{j=1}^{p}P(X_j=x_j \mid Y=y)
$$

---

## Clasificación con log-probabilidades

Regla de decisión:

$$
\hat{y}=\arg\max_y
\left[
\log P(Y=y)+
\sum_j \log P(X_j=x_j \mid Y=y)
\right]
$$

Motivo:

- Evita underflow.
- Convierte productos en sumas.
- Permite normalizar con log-sum-exp.

---

## Naive Bayes categórico

Para variables discretas:

$$
\hat{P}(X_j=v \mid Y=y)=
\frac{n_{jvy}+\alpha}{n_y+\alpha K_j}
$$

Donde:

- $\alpha$: suavizado de Laplace.
- $K_j$: número de categorías de la variable $j$.
- Evita probabilidades cero.

---

## Naive Bayes gaussiano

Para variables continuas:

$$
X_j \mid Y=c \sim N(\mu_{jc},\sigma_{jc}^2)
$$

Likelihood:

$$
P(x_j \mid y=c)=
\frac{1}{\sqrt{2\pi\sigma_{jc}^2}}
\exp\left(-\frac{(x_j-\mu_{jc})^2}{2\sigma_{jc}^2}\right)
$$

Se estima $\mu_{jc}$ y $\sigma_{jc}^2$ por clase y variable.

---

## Diagnósticos de Naive Bayes

Revisar:

- Log-loss: calidad probabilística.
- Accuracy: calidad de clasificación dura.
- Variables con fuga de información.
- Independencia condicional aproximada.
- Normalidad condicional en GaussianNB.
- Calibración si se usarán probabilidades para decisiones.

En Titanic, no usar `alive` para predecir `survived`.

---

<!-- _class: small -->

## Notebook de clase

`notebooks/02_probabilidad_condicional_total.ipynb`

Bloques:

1. Probabilidad condicional en dados.
2. Probabilidad total por partición.
3. Test médico y tasa base.
4. Bayes secuencial.
5. EVPI y EVSI.
6. Naive Bayes mínimo.
7. Titanic categórico desde cero.
8. Titanic gaussiano desde cero.

---

# Ejercicios

Los siguientes problemas cubren teoría, cálculo manual, decisión y código.

---

## Ejercicio 1: diagnóstico médico

Una enfermedad tiene prevalencia:

$$
P(D)=0.03
$$

La prueba tiene:

$$
P(+ \mid D)=0.92
$$

$$
P(- \mid D^c)=0.88
$$

Calcular:

1. $P(+)$ usando probabilidad total.
2. $P(D \mid +)$ usando Bayes.
3. Interpretación no técnica.

---

<!-- _class: small -->

## Solución 1

Datos:

$$
P(+ \mid D^c)=1-0.88=0.12
$$

Probabilidad total:

$$
P(+)=P(+ \mid D)P(D)+P(+ \mid D^c)P(D^c)
$$

$$
P(+)=0.92(0.03)+0.12(0.97)=0.144
$$

Posterior:

$$
P(D \mid +)=\frac{0.92(0.03)}{0.144}=0.1917
$$

Interpretación: un positivo implica aproximadamente 19.2% de probabilidad de enfermedad. La prevalencia baja modera el posterior.

---

## Ejercicio 2: EVPI con tres demandas

Estados:

| Estado | Probabilidad |
|---|---:|
| Demanda baja | 0.25 |
| Demanda media | 0.50 |
| Demanda alta | 0.25 |

Acciones: no lanzar, lanzar pequeño, lanzar grande.

Calcular:

1. Valor esperado por acción.
2. Mejor acción sin información.
3. Valor esperado con información perfecta.
4. EVPI.

---

<!-- _class: small -->

## Ejercicio 2: matriz de pagos

| Acción | Baja | Media | Alta |
|---|---:|---:|---:|
| No lanzar | 0 | 0 | 0 |
| Lanzar pequeño | -10 | 35 | 55 |
| Lanzar grande | -40 | 45 | 120 |

La matriz está alineada con el notebook de semana 2.

---

<!-- _class: small -->

## Solución 2: valor esperado

$$
EV(\text{no lanzar})=0
$$

$$
EV(\text{pequeño})=0.25(-10)+0.50(35)+0.25(55)=28.75
$$

$$
EV(\text{grande})=0.25(-40)+0.50(45)+0.25(120)=42.50
$$

Mejor acción sin información:

$$
\text{lanzar grande}, \qquad EV_{\text{sin info}}=42.50
$$

---

<!-- _class: small -->

## Solución 2: EVPI

Con información perfecta:

- Si baja: elegir no lanzar, pago 0.
- Si media: elegir lanzar grande, pago 45.
- Si alta: elegir lanzar grande, pago 120.

$$
EV_{\text{perfecta}}=0.25(0)+0.50(45)+0.25(120)=52.50
$$

$$
EVPI=52.50-42.50=10.00
$$

Interpretación: no pagar más de 10 unidades por información perfecta sobre demanda.

---

## Ejercicio 3: Titanic y Naive Bayes

Usar:

`data_sources/titanic.csv`

Tareas:

1. Construir versión categórica de `age` y `fare`.
2. Estimar $P(Y)$ y $P(X_j \mid Y)$ con Laplace.
3. Clasificar un pasajero hipotético.
4. Comparar con `sklearn.CategoricalNB`.

---

<!-- _class: small -->

## Solución 3: preparación

Objetivo:

$$
Y=\text{survived}
$$

Predictores usados:

- `pclass`
- `sex`
- `age_bin`
- `fare_bin`
- `embarked`
- `alone`
- `family_bin`

Variables excluidas:

- `alive`, porque replica el objetivo.
- identificadores o texto libre no tratados.

---

<!-- _class: small -->

## Solución 3: estimación

Prior:

$$
\hat{P}(Y=y)=\frac{n_y}{N}
$$

Likelihood categórico:

$$
\hat{P}(X_j=x \mid Y=y)=
\frac{n_{jxy}+\alpha}{n_y+\alpha K_j}
$$

Score:

$$
\log P(Y=y)+\sum_j\log P(X_j=x_j \mid Y=y)
$$

Predicción:

$$
\hat{y}=\arg\max_y \text{score}(y)
$$

---

<!-- _class: small -->

## Solución 3: pasajero hipotético

Pasajero:

- `pclass=3`
- `sex=male`
- `age=30`, entonces `age_bin=adulto joven`
- `fare` bajo
- `embarked=S`
- `alone=True`
- `family_bin=solo`

Con el modelo categórico entrenado sobre Titanic:

$$
P(\text{no sobrevive} \mid x)=0.9811
$$

$$
P(\text{sobrevive} \mid x)=0.0189
$$

Clasificación: no sobrevive.

---

<!-- _class: small -->

## Solución 3: comparación esperada

En el notebook:

| Modelo | Accuracy | Log-loss | Predicciones iguales |
|---|---:|---:|---|
| Naive Bayes categórico desde cero | 0.7130 | 0.6678 | True |
| `sklearn.CategoricalNB` | 0.7130 | 0.6678 | True |

Lectura:

La implementación propia replica la lógica de scikit-learn en este diseño categórico.

---

## Ejercicio 4: Bayes secuencial

Datos:

$$
P(D)=0.02
$$

$$
P(+ \mid D)=0.95
$$

$$
P(- \mid D^c)=0.90
$$

Dos pruebas positivas independientes condicionalmente al estado real.

Calcular $LR_+$, $P(D \mid +)$, $P(D \mid +,+)$ y explicar el supuesto.

---

<!-- _class: small -->

## Solución 4: primer positivo

Falso positivo:

$$
P(+ \mid D^c)=0.10
$$

Likelihood ratio positivo:

$$
LR_+=\frac{P(+ \mid D)}{P(+ \mid D^c)}=\frac{0.95}{0.10}=9.5
$$

Posterior con un positivo:

$$
P(D \mid +)=
\frac{0.95(0.02)}
{0.95(0.02)+0.10(0.98)}
=0.1624
$$

---

<!-- _class: small -->

## Solución 4: dos positivos

Actualización secuencial:

$$
P(D \mid +,+)=
\frac{0.95P(D \mid +)}
{0.95P(D \mid +)+0.10(1-P(D \mid +))}
$$

$$
P(D \mid +,+)=0.6480
$$

Con odds:

$$
\text{odds prior}=\frac{0.02}{0.98}=0.0204
$$

$$
\text{odds posterior}=0.0204(9.5)^2=1.8418
$$

$$
P(D \mid +,+)=\frac{1.8418}{1+1.8418}=0.6480
$$

---

## Solución 4: supuesto crítico

Multiplicar por $LR_+$ dos veces requiere:

$$
P(+_1,+_2 \mid D)=P(+_1 \mid D)P(+_2 \mid D)
$$

y también:

$$
P(+_1,+_2 \mid D^c)=P(+_1 \mid D^c)P(+_2 \mid D^c)
$$

Si las pruebas usan el mismo sensor, muestra o proceso, la independencia condicional puede fallar y el posterior queda sobreestimado.

---

## Ejercicio 5: información imperfecta

Estados:

| Estado | Probabilidad |
|---|---:|
| Demanda baja | 0.25 |
| Demanda media | 0.50 |
| Demanda alta | 0.25 |

Señal:

$$
S\in\{\text{desfavorable},\text{neutral},\text{favorable}\}
$$

Calcular $P(S)$, $P(\theta \mid S)$, política óptima, EVSI y valor neto.

---

<!-- _class: small -->

## Ejercicio 5: matriz $P(S \mid \theta)$

| Señal | Baja | Media | Alta |
|---|---:|---:|---:|
| Desfavorable | 0.65 | 0.20 | 0.10 |
| Neutral | 0.25 | 0.55 | 0.25 |
| Favorable | 0.10 | 0.25 | 0.65 |

Cada columna suma 1.

Pagos: los mismos del ejercicio 2.

Costo de la señal:

$$
c=8
$$

---

<!-- _class: small -->

## Solución 5: marginal de la señal

$$
P(S=s)=\sum_\theta P(S=s \mid \theta)P(\theta)
$$

Resultados:

| Señal | $P(S=s)$ |
|---|---:|
| Desfavorable | 0.2875 |
| Neutral | 0.4000 |
| Favorable | 0.3125 |

---

<!-- _class: small -->

## Solución 5: posteriores

| Señal | $P(\text{baja}\mid S)$ | $P(\text{media}\mid S)$ | $P(\text{alta}\mid S)$ |
|---|---:|---:|---:|
| Desfavorable | 0.5652 | 0.3478 | 0.0870 |
| Neutral | 0.1563 | 0.6875 | 0.1563 |
| Favorable | 0.0800 | 0.4000 | 0.5200 |

Bayes se aplica fila por fila:

$$
P(\theta \mid S=s)=\frac{P(S=s \mid \theta)P(\theta)}{P(S=s)}
$$

---

<!-- _class: small -->

## Solución 5: política por señal

| Señal | Mejor acción | Valor posterior |
|---|---|---:|
| Desfavorable | Lanzar pequeño | 11.3043 |
| Neutral | Lanzar grande | 43.4375 |
| Favorable | Lanzar grande | 77.2000 |

Valor esperado con señal:

$$
EV_{\text{con señal}}
=0.2875(11.3043)+0.4000(43.4375)+0.3125(77.2)
=44.75
$$

---

<!-- _class: small -->

## Solución 5: EVSI y valor neto

Del ejercicio 2:

$$
EV_{\text{sin info}}=42.50
$$

Entonces:

$$
EVSI=44.75-42.50=2.25
$$

Comparación:

$$
0\le 2.25\le 10.00=EVPI
$$

Valor neto con costo 8:

$$
2.25-8=-5.75
$$

Conclusión: la señal informa, pero no conviene pagarla a ese costo.

---

# Cierre

La sesión conecta:

- Condicionar como cambio de universo.
- Probabilidad total como marginalización.
- Bayes como actualización coherente.
- Valor de información como decisión económica.
- Naive Bayes como algoritmo derivado de Bayes.

---

## Checklist de entrega

En cualquier caso aplicado, exigir:

- Eventos, hipótesis y partición definidos con claridad.
- Priors y likelihoods en tabla.
- Denominador calculado con probabilidad total.
- Posterior interpretado en lenguaje del caso.
- Sensibilidad a priors o tasas de error.
- Separación entre EVPI, EVSI y costo de información.
- Diagnósticos de modelo si se estiman probabilidades con datos.

---

## Material asociado

- Teoría: `docs/sesion_02_probabilidad_condicional_total.md`
- Notebook: `notebooks/02_probabilidad_condicional_total.ipynb`
- Ejercicios: `ejercicios/sesion_02.md`
- Soluciones: `soluciones/sesion_02.md`
- Dataset Titanic: `data_sources/titanic.csv`
