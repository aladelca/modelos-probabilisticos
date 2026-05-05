# Sesión 01 - Conceptos básicos de probabilidad

## Logro de la sesión

Al finalizar la sesión, el estudiante identifica experimentos aleatorios, construye espacios muestrales, representa eventos y evalúa independencia usando reglas de probabilidad.

## Conceptos clave

Un **experimento aleatorio** es un proceso cuyo resultado no se conoce antes de observarlo, aunque sí se puede describir el conjunto de resultados posibles. Ese conjunto es el **espacio muestral** $\Omega$.

Un **evento** es un subconjunto de $\Omega$. Si el resultado observado pertenece al evento, decimos que el evento ocurrió.

Operaciones frecuentes:

- Complemento: $A^c$, ocurre cuando no ocurre $A$.
- Unión: $A \cup B$, ocurre si ocurre al menos uno.
- Intersección: $A \cap B$, ocurre si ocurren ambos.
- Diferencia: $A \setminus B$, ocurre $A$ pero no $B$.

## Axiomas y reglas

Una función de probabilidad $P$ debe cumplir:

$$
P(A) \ge 0,\quad P(\Omega)=1,\quad P(A\cup B)=P(A)+P(B)
$$

cuando $A$ y $B$ son mutuamente excluyentes.

Consecuencias útiles:

$$
P(A^c)=1-P(A)
$$

$$
P(A\cup B)=P(A)+P(B)-P(A\cap B)
$$

Si todos los resultados elementales son equiprobables:

$$
P(A)=\frac{|A|}{|\Omega|}
$$

## Independencia

Dos eventos $A$ y $B$ son independientes si saber que ocurrió uno no cambia la probabilidad del otro:

$$
P(A\cap B)=P(A)P(B)
$$

Equivalente, si $P(B)>0$:

$$
P(A\mid B)=P(A)
$$

No confundir independencia con exclusión mutua. Si dos eventos no vacíos son mutuamente excluyentes, entonces $P(A\cap B)=0$, por lo que normalmente no son independientes.

## Ejemplo guía

Experimento: lanzar dos dados.

$$
\Omega=\{(i,j): i,j\in\{1,2,3,4,5,6\}\}
$$

Eventos:

- $A$: la suma es par.
- $B$: el primer dado es par.
- $C$: la suma es mayor o igual que 10.

Preguntas:

1. Calcular $P(A)$, $P(B)$, $P(A\cap B)$.
2. Evaluar si $A$ y $B$ son independientes.
3. Calcular $P(A\cup C)$.

## Errores comunes

- Usar conteo equiprobable cuando los resultados no tienen la misma probabilidad.
- Decir que dos eventos son independientes solo porque parecen distintos.
- Olvidar restar $P(A\cap B)$ al calcular $P(A\cup B)$.
- Confundir el resultado elemental con una variable numérica derivada del resultado.

## Actividad formativa

Diseña un experimento aleatorio simple en un contexto real: compras online, transporte, salud, deporte o atención al cliente. Define $\Omega$, tres eventos, sus operaciones principales y una hipótesis de independencia. Luego valida la hipótesis con una simulación en Python.

## Profundización para nivel de maestría

### Espacios muestrales finitos, numerables y continuos

En problemas introductorios se suele trabajar con espacios muestrales finitos, como dados o cartas. En modelado probabilístico aplicado, $\Omega$ puede ser mucho más complejo:

- Finito: clase predicha por un modelo, estado de un cliente, resultado de un test.
- Numerable: número de visitas hasta conversión, reclamos por día, defectos por lote.
- Continuo: tiempo de vida, monto de compra, ubicación espacial, temperatura.
- Producto: una secuencia de observaciones $(X_1,\ldots,X_n)$, una serie temporal o un vector de features.

La idea central no cambia: un evento sigue siendo un subconjunto de resultados posibles. Lo que cambia es la herramienta matemática para asignar probabilidad: conteo en espacios finitos, sumas en espacios numerables e integrales en espacios continuos.

### Álgebra de eventos y sigma-álgebra

Para un curso aplicado basta pensar en eventos como subconjuntos de $\Omega$. En una formulación más rigurosa se define una colección $\mathcal{F}$ de eventos permitidos, llamada sigma-álgebra. Debe cumplir:

$$
\Omega \in \mathcal{F}, \quad A\in\mathcal{F}\Rightarrow A^c\in\mathcal{F}, \quad A_1,A_2,\ldots\in\mathcal{F}\Rightarrow \bigcup_i A_i\in\mathcal{F}
$$

Esta estructura garantiza que las operaciones usuales entre eventos produzcan eventos válidos. Es importante en variables continuas porque no todos los subconjuntos arbitrarios son medibles de forma útil.

### Formas de asignar probabilidades

En aplicaciones reales no siempre hay simetría.

| Enfoque | Idea | Ejemplo |
|---|---|---|
| Clásico | Conteo de casos equiprobables | Dados justos, barajas bien mezcladas |
| Frecuentista | Límite de frecuencia relativa | Tasa histórica de churn |
| Subjetivo/Bayesiano | Grado de creencia coherente | Riesgo de lanzamiento de un producto nuevo |
| Model-based | Probabilidad inducida por un modelo paramétrico | $X\sim Poisson(\lambda)$, $Y\sim N(\mu,\sigma^2)$ |

Una entrega de maestría debe explicitar cuál enfoque se está usando y por qué es razonable para el caso.

### Conteo más allá de casos simples

Cuando $\Omega$ es finito y equiprobable:

$$
P(A)=\frac{|A|}{|\Omega|}
$$

El reto está en contar correctamente. Herramientas frecuentes:

- Permutaciones: el orden importa.
- Combinaciones: el orden no importa.
- Regla multiplicativa: decisiones secuenciales independientes en conteo.
- Inclusión-exclusión: evita doble conteo en uniones.

Para dos eventos:

$$
P(A\cup B)=P(A)+P(B)-P(A\cap B)
$$

Para tres:

$$
P(A\cup B\cup C)=P(A)+P(B)+P(C)-P(A\cap B)-P(A\cap C)-P(B\cap C)+P(A\cap B\cap C)
$$

### Independencia por pares vs independencia conjunta

En problemas reales no basta revisar pares. Una colección $A_1,\ldots,A_k$ es mutuamente independiente si para todo subconjunto de índices $I$:

$$
P\left(\bigcap_{i\in I}A_i\right)=\prod_{i\in I}P(A_i)
$$

La independencia por pares no garantiza independencia conjunta. Esto importa al construir modelos con supuestos tipo "features independientes", porque una validación solo por correlaciones o pares puede ocultar dependencias de orden superior.

### Independencia condicional

Otra noción clave es independencia condicional:

$$
A\perp B\mid C
$$

que significa:

$$
P(A\cap B\mid C)=P(A\mid C)P(B\mid C)
$$

En ciencia de datos aparece en Naive Bayes, modelos gráficos, inferencia causal y segmentación. Dos variables pueden ser dependientes marginalmente pero independientes dentro de cada segmento, o al revés. Esta es una fuente común de interpretaciones erróneas.

### Simulación como verificación conceptual

La simulación Monte Carlo no reemplaza la teoría; ayuda a comprobar intuiciones, depurar fórmulas y comunicar resultados. Un buen flujo es:

1. Definir $\Omega$ y eventos de forma exacta.
2. Calcular probabilidades analíticas cuando sea posible.
3. Simular bajo los mismos supuestos.
4. Comparar error empírico vs tamaño muestral.
5. Discutir qué supuestos del simulador serían falsos en un caso real.

### Criterios de calidad para informes

Un análisis de eventos está bien planteado si:

- El experimento aleatorio está descrito sin ambigüedad.
- El espacio muestral es compatible con el problema.
- Los eventos están definidos como subconjuntos o condiciones verificables.
- La asignación de probabilidades tiene justificación.
- Se distingue entre eventos excluyentes, independientes y condicionados.
- Las conclusiones no sobreinterpretan una simulación finita.
