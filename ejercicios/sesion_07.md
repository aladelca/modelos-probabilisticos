# Ejercicios - Sesión 07

## Ejercicio 1: modelos discretos básicos

Para cada caso, elige un modelo y justifica:

1. Conversión sí/no de un usuario.
2. Número de conversiones en 200 visitas.
3. Número de llamadas a servicio por cliente.
4. Categoría elegida entre web, tienda, app o call center.

## Ejercicio 2: churn y pérdidas

Usa `telecom_churn.csv`.

1. Entrena una regresión logística estándar.
2. Entrena una regresión logística con `class_weight="balanced"`.
3. Compara log-loss, Brier, ROC-AUC y PR-AUC.
4. Busca el umbral que minimiza costo con $C_{FN}=6$ y $C_{FP}=1$.

## Ejercicio 3: focal loss

Entrena CatBoost con `Logloss` y con `Focal:focal_alpha=0.75;focal_gamma=2`.

1. Compara log-loss y PR-AUC.
2. Evalúa focal loss como métrica.
3. Explica cuándo focal loss mejora o empeora calibración.
