# Soluciones guía - Casos integradores

## Unidad 1

Eventos:

- $I$: cliente interesado.
- $S+$: score positivo.

Datos:

$$
P(I)=0.12,\quad P(S+\mid I)=0.80,\quad P(S+\mid I^c)=0.25
$$

Probabilidad total:

$$
P(S+)=0.80(0.12)+0.25(0.88)=0.316
$$

Posterior:

$$
P(I\mid S+)=\frac{0.80(0.12)}{0.316}=0.3038
$$

El score aumenta la probabilidad de interés de 12% a 30.4%. Para valorar si conviene, se debe comparar utilidad esperada con y sin score, descontando costo de contacto y costo del modelo.

## Unidad 2

La variable principal puede ser:

$$
D_{s,p,t}=Demand\text{ del producto }p\text{ en tienda }s\text{ en día }t
$$

Una solución sólida debe reportar:

- Media, mediana, varianza, CV, asimetría y percentiles.
- $E[D\mid Promotion]$ y $E[D\mid Promotion,Weather]$.
- Matriz de covarianza para `Demand`, `Units Sold`, `Price`, `Discount`, `Promotion`.
- Verificación de Schwarz en al menos dos pares.
- Autocorrelación de una serie producto-tienda.

Conclusión operativa esperada: usar percentiles o cuantiles condicionales para inventario cuando la demanda es asimétrica o segmentada por promoción/clima.

## Unidad 3

La solución depende del dataset elegido, pero debe tener esta estructura mínima:

1. Variable aleatoria y soporte.
2. Modelos candidatos compatibles con soporte.
3. Estimación de parámetros o entrenamiento.
4. Validación con métricas coherentes.
5. Simulación de escenarios.
6. Probabilidad o cuantil de decisión.
7. Sensibilidad ante supuestos.

Ejemplo para tráfico:

- Variable: $T=$ duración en tráfico.
- Modelos: Exponencial, Gamma, Lognormal.
- Validación: KS, CDF empírica, cola superior.
- Decisión: $P(T>20)$ o percentil 90 para planificación.
