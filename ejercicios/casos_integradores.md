# Casos integradores por unidad

## Unidad 1: diagnóstico y marketing con Bayes

Una empresa vende un servicio premium. Antes de una campaña, estima que 12% de clientes están realmente interesados. Un modelo de scoring marca positivo al 80% de los interesados y al 25% de los no interesados.

1. Define eventos e hipótesis.
2. Calcula la probabilidad total de score positivo.
3. Calcula $P(Interesado\mid Score+)$.
4. Supón acciones `no contactar`, `email`, `llamada`. Construye una matriz de pagos y calcula la mejor acción sin información.
5. Calcula el valor esperado si se usa el score para decidir acción.
6. Discute cuándo el score no valdría la pena.

## Unidad 2: demanda, momentos y covarianza

Usa `sales_data.csv`.

1. Define la variable aleatoria principal para demanda.
2. Calcula momentos y percentiles de `Demand`.
3. Estima expectativas condicionales por promoción, clima y categoría.
4. Construye la matriz de covarianza de variables numéricas.
5. Verifica Cauchy-Schwarz para dos pares.
6. Construye una serie de demanda por producto y revisa autocorrelación.
7. Propón una conclusión operativa para inventario.

## Unidad 3: modelo probabilístico final

Escoge una variable de `sales_data.csv`, `telecom_churn.csv` o tráfico.

1. Define la pregunta de decisión.
2. Define variable aleatoria y soporte.
3. Propón al menos tres modelos candidatos.
4. Estima parámetros o entrena modelos.
5. Valida forma, calibración o error predictivo según corresponda.
6. Simula escenarios.
7. Calcula probabilidades o cuantiles accionables.
8. Discute sensibilidad y limitaciones.
