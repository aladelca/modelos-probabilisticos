# Mapa del curso

Este mapa convierte el sílabo en una ruta de trabajo para repositorio: cada sesión tiene un apunte teórico y un notebook práctico.

## Convenciones

- Los notebooks están numerados `01` a `08` para evitar ambigüedad de orden.
- Cada notebook usa datos sintéticos o espacios muestrales generados en código, y varios activan bloques con `data_sources/` si la carpeta existe.
- Las actividades están pensadas para entregar como informe breve, no como presentación.
- El material previo está preservado en `legacy/material_original/` y no se trackea.
- Las fuentes de datos reutilizables están identificadas en `docs/fuentes_datos.md`; los archivos viven en `data_sources/`, carpeta ignorada por git.

## Cobertura por unidad

| Unidad | Sesiones | Resultado esperado | Evidencia sugerida |
|---|---:|---|---|
| 1. Conceptos básicos de probabilidad | 01-02 | Analizar asignación de probabilidades en eventos bajo distintas condiciones | Informe con eventos, independencia, probabilidad condicional y probabilidad total |
| 2. Variables aleatorias | 03-06 | Evaluar distribuciones, momentos y relaciones entre variables | Notebook con PMF/PDF/CDF, momentos, covarianza y desigualdad de Schwarz |
| 3. Modelos de probabilidad | 07-08 | Elaborar modelos probabilísticos discretos y continuos para casos reales | Modelo con supuestos explícitos, ajuste, simulación, validación y lectura de riesgo |

## Secuencia mínima de aprendizaje

1. Definir el experimento: qué se observa, cuál es el espacio muestral y qué eventos importan.
2. Asignar probabilidades: por simetría, frecuencia, juicio informado o modelo paramétrico.
3. Condicionar: actualizar probabilidades al observar nueva información.
4. Convertir resultados en variables aleatorias: pasar de eventos a funciones numéricas.
5. Describir distribuciones: PMF, PDF, CDF y cuantiles.
6. Resumir con momentos: media, varianza, asimetría, curtosis, covarianza y correlación.
7. Elegir modelos: Bernoulli, Binomial, Poisson, Multinomial, Normal, Exponencial, Lognormal, Gamma.
8. Validar el modelo: comparar simulación, datos observados, supuestos y métricas.

## Expectativa de nivel maestría

El curso no debe quedarse en sustitución mecánica de fórmulas. En cada sesión se espera que el estudiante pueda:

- Formular la variable o evento de interés con precisión.
- Declarar supuestos y reconocer cuándo son discutibles.
- Derivar o justificar la fórmula usada, al menos a nivel operativo.
- Implementar una simulación reproducible en Python.
- Comparar resultado analítico, simulación y lectura de negocio.
- Explicar sensibilidad ante parámetros, priors, colas, segmentación o tamaño muestral.
- Proponer una alternativa cuando el modelo base falla.

## Plantilla de análisis por sesión

Para que los informes mantengan un estándar común, cada caso puede escribirse con esta estructura:

1. **Pregunta**: decisión o fenómeno que se quiere responder.
2. **Unidad de análisis**: cliente, transacción, día, producto, ticket, lote.
3. **Objeto probabilístico**: evento, variable aleatoria o vector aleatorio.
4. **Modelo o regla**: axiomas, condicional, PMF/PDF, momentos o distribución paramétrica.
5. **Supuestos**: independencia, equiprobabilidad, tasa constante, soporte, estacionariedad, etc.
6. **Cálculo**: fórmula, implementación y simulación si corresponde.
7. **Validación**: comparación empírica, gráficos, sensibilidad o diagnóstico.
8. **Interpretación**: qué significa el resultado para la decisión.
9. **Limitaciones**: qué podría invalidar el análisis.

## Entregables recomendados

| Entregable | Sesiones base | Contenido mínimo |
|---|---:|---|
| CL1 | 01 | Espacio muestral, eventos, operaciones, independencia |
| CL2 / TB1 | 02 | Condicional, probabilidad total, Bayes como extensión |
| CL3 | 03 | Variable aleatoria discreta vs continua |
| CL4 | 04 | PMF/PDF/CDF y probabilidades por rango |
| CL5 | 05 | Momentos, posición, dispersión, expectativa condicional |
| CL6 / TB2 | 06 | Momentos conjuntos, matriz de covarianza, Schwarz |
| CL7 | 07 | Modelos discretos y selección por supuestos |
| CL8 / TF | 08 | Modelos continuos, TLC y modelo final documentado |

## Alineación con el material original

| Material legacy | Cobertura nueva |
|---|---|
| `clase1_conceptos_numpy.ipynb`, `caso_fenomeno_aleatorio.py`, `caso_eventos_en_fenomeno_aleatorio.py` | Sesión 01: dados, eventos, álgebra, independencia, Bernoulli y DNI |
| `clase.ipynb`, `probabilidad_condicional_*.py`, `valor_informacion_ejercicios.py`, `naive_bayes_titanic.py`, `naive_bayes_gauss_titanic.py` | Sesión 02: condicional, probabilidad total, Bayes, EVPI, Naive Bayes didáctico y Titanic desde cero |
| `variables_aleatorias.ipynb` vacío y contenido conceptual disperso | Sesión 03 reconstruida completa: variable aleatoria discreta, continua, CDF y transformaciones |
| `notebooks/S4_ejemplo/demand.ipynb`, `modelos_demanda.ipynb 3` | Sesiones 04 y 08: distribución empírica, demanda, modelos y validación |
| `guion 3.md`, `sesion5_momentos_caso_real.ipynb 3`, `estadisticos_posicion_deep_learning.ipynb 3` | Sesión 05: momentos, posición, dispersión, expectativa condicional y lectura avanzada |
| `guion_momentos_dos_variables.md`, `aplicacion_momentos_dos_variables.ipynb` | Sesión 06: momentos conjuntos, Schwarz, covarianza, PCA, autocovarianza, ARIMA, ARIMAX y VAR |
| `guion 4.md`, `taller_modelos_discretos.ipynb`, `telecom_churn.csv`, `results_multinomial.csv` | Sesión 07: Bernoulli, Binomial, Poisson, NegBin, Multinomial, churn, multiclase, log-loss y focal loss |
| `modelos_variables_continuas.ipynb 3`, `modelos_demanda.ipynb 3`, datos de tráfico | Sesión 08: modelos continuos, ajuste de distribuciones, TLC/bootstrap y pérdidas para demanda |
