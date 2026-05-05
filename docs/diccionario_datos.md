# Diccionario de datos

Este documento describe las fuentes en `data_sources/` desde una perspectiva probabilística: unidad de análisis, columnas, variables aleatorias posibles, supuestos útiles, limitaciones y sesiones donde se usan.

## `sales_data.csv`

**Unidad de análisis:** combinación fecha-tienda-producto.

**Uso principal:** demanda, ventas, precios, promociones, clima, momentos, covarianza, series de tiempo y pérdidas de predicción.

| Columna | Tipo | Lectura probabilística |
|---|---|---|
| `Date` | fecha | Índice temporal; permite definir procesos estocásticos $X_t$. |
| `Store ID` | categórica | Segmento o efecto de tienda. |
| `Product ID` | categórica | Segmento de producto; permite series por SKU. |
| `Category` | categórica | Variable condicionante para demanda y precio. |
| `Region` | categórica | Segmento geográfico. |
| `Inventory Level` | conteo amplio | Stock disponible; covariable para demanda observada y ventas. |
| `Units Sold` | conteo | Variable aleatoria discreta; ventas realizadas. |
| `Units Ordered` | conteo | Reposición o pedido; variable de decisión/operación. |
| `Price` | continua positiva | Precio; variable continua, posible covariable en demanda. |
| `Discount` | discreta/porcentaje | Intensidad promocional; covariable. |
| `Weather Condition` | categórica | Estado externo para expectativa condicional. |
| `Promotion` | Bernoulli | Indicador de campaña. |
| `Competitor Pricing` | continua positiva | Precio externo; covariable. |
| `Seasonality` | categórica | Estado temporal agregado. |
| `Epidemic` | Bernoulli | Indicador de shock externo. |
| `Demand` | conteo amplio | Variable objetivo; puede modelarse como conteo, continua positiva aproximada o serie temporal. |

**Variables aleatorias sugeridas:**

- $D$: demanda por producto-tienda-día.
- $S$: unidades vendidas por día.
- $P$: precio observado.
- $I(D>q_{0.9})$: indicador de demanda alta.
- $D_t$ para una serie temporal de demanda por producto.

**Supuestos que pueden fallar:**

- Independencia entre observaciones: hay dependencia temporal y por producto.
- Estacionariedad: promociones, clima y estacionalidad cambian la distribución.
- Demanda observada vs demanda real: stock bajo puede censurar ventas.
- Normalidad: `Demand` y `Units Sold` son conteos y pueden ser asimétricos.

**Diagnósticos recomendados:**

- Histograma, CDF empírica y cuantiles de `Demand`.
- Media-varianza para revisar Poisson vs sobredispersión.
- $E[Demand \mid Promotion, Weather]$.
- Matriz de correlación/covarianza.
- Autocorrelación por producto-tienda.

**Sesiones:** 03, 04, 05, 06, 08.

## `telecom_churn.csv`

**Unidad de análisis:** cliente de telecomunicaciones.

**Uso principal:** Bernoulli, churn, conteos de llamadas, pérdidas de clasificación y costos asimétricos.

| Columna | Tipo | Lectura probabilística |
|---|---|---|
| `Churn` | Bernoulli | Variable objetivo $Y\in\{0,1\}$. |
| `AccountWeeks` | conteo/edad | Antigüedad de cuenta. |
| `ContractRenewal` | Bernoulli | Indicador de renovación. |
| `DataPlan` | Bernoulli | Indicador de plan de datos. |
| `DataUsage` | continua no negativa | Uso de datos. |
| `CustServCalls` | conteo | Llamadas a servicio; candidato Poisson/NegBin. |
| `DayMins` | continua no negativa | Minutos diurnos. |
| `DayCalls` | conteo | Número de llamadas diurnas. |
| `MonthlyCharge` | continua/discreta positiva | Cargo mensual. |
| `OverageFee` | continua no negativa | Cargo por exceso. |
| `RoamMins` | continua no negativa | Minutos roaming. |

**Variables aleatorias sugeridas:**

- $Y$: churn.
- $C$: llamadas a servicio.
- $M$: cargo mensual.
- $P(Y=1\mid X)$: probabilidad condicional de churn.

**Supuestos que pueden fallar:**

- Independencia entre clientes si hay campañas o shocks regionales.
- Calibración de probabilidades en clases desbalanceadas.
- Poisson para `CustServCalls` si hay sobredispersión.

**Diagnósticos recomendados:**

- Tasa base de churn.
- Curvas ROC/PR, log-loss y Brier score.
- Matriz de confusión por umbral.
- Media-varianza de `CustServCalls`.
- Comparación log-loss vs focal loss.

**Sesiones:** 07.

## `titanic.csv`

**Unidad de análisis:** pasajero.

**Uso principal:** Naive Bayes categórico y Gaussiano desde cero.

| Columna | Tipo | Lectura probabilística |
|---|---|---|
| `survived` | Bernoulli | Variable objetivo. |
| `pclass` | ordinal/categórica | Clase del ticket. |
| `sex` | categórica | Predictor categórico. |
| `age` | continua positiva | Predictor continuo con faltantes. |
| `sibsp` | conteo | Familiares directos a bordo. |
| `parch` | conteo | Padres/hijos a bordo. |
| `fare` | continua positiva | Tarifa pagada. |
| `embarked` | categórica | Puerto de embarque. |
| `class`, `who`, `adult_male`, `deck`, `embark_town`, `alive`, `alone` | categóricas/binarias | Variables descriptivas; algunas duplican información de otras. |

**Variables aleatorias sugeridas:**

- $Y$: supervivencia.
- $X_j$: predictor categórico discretizado.
- $A$: edad como variable continua bajo verosimilitud Gaussiana.

**Supuestos que pueden fallar:**

- Independencia condicional entre predictores de Naive Bayes.
- Normalidad de `age` y `fare` dentro de cada clase.
- Sesgo por valores faltantes (`age`, `deck`, `embarked`).
- Variables duplicadas (`alive` equivale a `survived` y no debe usarse como predictor).

**Diagnósticos recomendados:**

- Comparar implementación propia vs `sklearn`.
- Revisar log-loss, no solo accuracy.
- Tablas condicionales $P(X_j\mid Y)$.
- Histogramas de variables continuas por clase.

**Sesiones:** 02.

## `results_multinomial.csv`

**Unidad de análisis:** resultado agregado por modelo.

**Uso principal:** comparación de modelos multiclase.

| Columna | Tipo | Lectura probabilística |
|---|---|---|
| `model` | categórica | Algoritmo evaluado. |
| `accuracy` | proporción | Frecuencia de acierto. |
| `logloss` | continua positiva | Pérdida probabilística multiclase. |

**Variables aleatorias sugeridas:**

- $L$: log-loss por modelo.
- $A$: accuracy por modelo.

**Limitaciones:**

- Es una tabla de resultados, no datos crudos.
- No permite recalcular métricas por observación.
- Sirve para discutir scoring rules y tradeoffs, no para inferir causalidad.

**Sesiones:** 07.

## `resultados_futuro/*.csv`

**Unidad de análisis:** consulta de duración de ruta en intervalos de 30 minutos.

**Uso principal:** series de tiempo, autocovarianza, modelos continuos y TLC/bootstrap.

| Columna | Tipo | Lectura probabilística |
|---|---|---|
| `timestamp_local` | fecha-hora | Índice temporal. |
| `status` | categórica | Estado de consulta. |
| `duracion_min` | continua positiva | Duración estimada total. |
| `duracion_en_trafico_min` | continua positiva | Duración con tráfico; variable principal. |
| `duracion_sin_trafico_min` | continua positiva | Duración base. |
| `distancia_km` | continua positiva | Distancia de ruta. |
| `origen` | categórica | Punto de origen. |
| `destino` | categórica | Punto de destino. |

**Variables aleatorias sugeridas:**

- $T$: duración en tráfico.
- $T_t$: proceso temporal de duración.
- $\bar{T}_n$: promedio muestral para TLC.
- $I(T>c)$: incumplimiento de umbral.

**Supuestos que pueden fallar:**

- Independencia: mediciones consecutivas están autocorrelacionadas.
- Estacionariedad: hora, día y eventos externos cambian la distribución.
- Representatividad: una ruta específica no generaliza a todo Lima.

**Diagnósticos recomendados:**

- Histograma y CDF empírica de duración.
- Autocorrelación por rezago.
- Comparación Gamma/Lognormal/Exponencial.
- Bootstrap de promedios.

**Sesiones:** 06, 08.

## `trafico_lima_lunes6pm.csv`

**Unidad de análisis:** observación puntual de duración de ruta.

**Uso principal:** motivación de variable continua y comparación con la serie completa.

| Columna | Tipo | Lectura probabilística |
|---|---|---|
| `timestamp` | fecha-hora | Momento de consulta. |
| `duracion_min` | continua positiva | Duración observada. |

**Limitaciones:**

- Una sola observación no permite estimar una distribución.
- Sirve como caso motivador, no como base inferencial.

**Sesiones:** 08.

## `productos_falabella.parquet`

**Unidad de análisis:** producto.

**Uso principal:** fuente opcional para precios, montos positivos y análisis de colas.

**Variables aleatorias sugeridas:**

- $P$: precio original.
- $I(P>q_{0.9})$: indicador de producto caro.
- Texto o categoría como variables condicionantes.

**Limitaciones:**

- Requiere `pyarrow` o `fastparquet` para lectura.
- Puede contener columnas textuales o rutas a imágenes no incluidas.
- Es opcional; los notebooks centrales no dependen de este archivo.

**Sesiones:** 04, 05, 08.
