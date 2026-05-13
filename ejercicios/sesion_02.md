# Ejercicios - Sesión 02

## Ejercicio 1: diagnóstico médico

Una enfermedad tiene prevalencia $P(D)=0.03$. Una prueba tiene sensibilidad $P(+\mid D)=0.92$ y especificidad $P(-\mid D^c)=0.88$.

Calcula:

1. $P(+)$ usando probabilidad total.
2. $P(D\mid +)$ usando Bayes.
3. La interpretación en lenguaje no técnico.

## Ejercicio 2: valor esperado de información

Una empresa debe elegir entre lanzar un producto grande, pequeño o no lanzar. Los estados de demanda son baja, media y alta con probabilidades 0.25, 0.50 y 0.25. Define una matriz de pagos propia.

Calcula:

1. Valor esperado por acción.
2. Mejor acción sin información.
3. Valor esperado con información perfecta.
4. EVPI.

## Ejercicio 3: Titanic y Naive Bayes

Usa `data_sources/titanic.csv`.

1. Construye una versión categórica de `age` y `fare`.
2. Estima $P(Y)$ y $P(X_j\mid Y)$ con suavizado de Laplace.
3. Clasifica un pasajero hipotético.
4. Compara con `sklearn.CategoricalNB`.

## Ejercicio 4: actualización bayesiana secuencial

Una enfermedad tiene prevalencia $P(D)=0.02$. Dos pruebas independientes condicionalmente al estado real tienen sensibilidad 0.95 y especificidad 0.90.

Calcula:

1. El likelihood ratio positivo $LR_+=P(+\mid D)/P(+\mid D^c)$.
2. $P(D\mid +)$ usando Bayes.
3. $P(D\mid +,+)$ usando actualización secuencial.
4. $P(D\mid +,+)$ usando odds.
5. Qué supuesto justificaría o invalidaría multiplicar dos veces por $LR_+$.

## Ejercicio 5: valor de información imperfecta

Usa los estados de demanda baja, media y alta con probabilidades 0.25, 0.50 y 0.25. Propón tres acciones y una matriz de pagos.

Luego define una señal de mercado $S\in\{\text{desfavorable},\text{neutral},\text{favorable}\}$ con una matriz $P(S\mid \theta)$.

Calcula:

1. La probabilidad marginal de cada señal.
2. Los posteriores $P(\theta\mid S=s)$.
3. La mejor acción para cada señal.
4. El valor esperado con señal.
5. El EVSI y su comparación contra el EVPI.
6. El valor neto si adquirir la señal tiene costo operativo.
