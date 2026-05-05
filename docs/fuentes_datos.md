# Fuentes de datos del curso

La carpeta `data_sources/` contiene datasets reutilizados del material original. Está ignorada por git para evitar subir datos, resultados generados o archivos pesados. Este documento sí queda trackeado para identificar qué fuente corresponde a cada sesión.

## Inventario

| Archivo en `data_sources/` | Origen legacy | Uso recomendado | Sesiones |
|---|---|---|---|
| `sales_data.csv` | `legacy/material_original/sales_data 4.csv` y copia equivalente en `notebooks/S4_ejemplo/sales_data.csv` | Demanda, precio, promociones, clima, distribuciones empíricas, momentos, covarianza, PCA y modelos de demanda | 03, 04, 05, 06, 08 |
| `titanic.csv` | `seaborn.load_dataset("titanic")`, equivalente al dataset usado por los scripts legacy de Naive Bayes | Clasificación de supervivencia con Naive Bayes categórico y gaussiano desde cero | 02 |
| `telecom_churn.csv` | `legacy/material_original/telecom_churn.csv` | Bernoulli para churn, conteos de llamadas a servicio, sobredispersión, comparación Poisson vs Binomial Negativa | 07 |
| `results_multinomial.csv` | `legacy/material_original/results_multinomial.csv` | Comparación de modelos multiclase con accuracy y log-loss | 07 |
| `trafico_lima_lunes6pm.csv` | `legacy/material_original/trafico_lima_lunes6pm.csv` | Observación puntual de duración de tráfico; útil como motivación de variable continua | 08 |
| `resultados_futuro/*.csv` | `legacy/material_original/resultados_futuro/*.csv` | Series de duración de tráfico cada 30 minutos; autocovarianza, modelos continuos y TLC/bootstrap | 06, 08 |
| `productos_falabella.parquet` | `legacy/material_original/productos_falabella.parquet 3` | Dataset exploratorio de productos; fuente opcional para casos de precios o EDA probabilístico | 04, 05, 08 |

## Archivo excluido

`legacy/material_original/proyectodata-348005-88764640e119.json` no fue copiado a `data_sources/` porque parece una credencial de servicio, no una fuente de datos de clase. Debe permanecer fuera del repo trackeado.

## Convención de uso en notebooks

Los notebooks buscan los datos en:

1. `data_sources/`, si se ejecutan desde la raíz del repo.
2. `../data_sources/`, si se ejecutan desde la carpeta `notebooks/`.

Si la carpeta no existe, los notebooks siguen funcionando con datos sintéticos reproducibles.

## Mapeo con sesiones

| Sesión | Dataset externo opcional | Bloque de código |
|---:|---|---|
| 01 | Ninguno | Bernoulli, dados y DNI con simulación |
| 02 | `titanic.csv` | Probabilidad condicional, Bayes, EVPI, Naive Bayes didáctico, Naive Bayes categórico y Gaussiano desde cero |
| 03 | `sales_data.csv` | Clasificación de variables aleatorias desde columnas reales |
| 04 | `sales_data.csv`, `productos_falabella.parquet` opcional | PMF/PDF/CDF empíricas |
| 05 | `sales_data.csv` | Momentos, percentiles y expectativa condicional |
| 06 | `sales_data.csv`, `resultados_futuro/*.csv` | Covarianza, correlación, Cauchy-Schwarz, autocovarianza, AR(1), ARIMA, ARIMAX y VAR |
| 07 | `telecom_churn.csv`, `results_multinomial.csv` | Bernoulli, conteos, sobredispersión, multiclase, log-loss, costos asimétricos y focal loss |
| 08 | `resultados_futuro/*.csv`, `sales_data.csv` opcional | Modelos continuos, ajuste, TLC/bootstrap y pérdidas de demanda |
